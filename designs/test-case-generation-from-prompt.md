# Design: Test Case Generation from Prompt

## Overview

This feature allows users to create one or more test cases by providing a simple natural language prompt (e.g., "Test that a user can add items to cart and checkout"). The system generates complete test case specifications using LLM, shows a preview for user review/editing, and then persists the test cases.

This mirrors the existing `UseCasePromptModal` pattern but adapted for test cases, with support for generating multiple test cases from a single prompt when the scope warrants it.

## Current Architecture Context

### Existing Use Case from Prompt Pattern

```
User Instruction → UseCaseFromPromptAgent (LLM) → Preview → User Edits → onAdd() → Persist
```

Components involved:
- **Frontend**: `UseCasePromptModal.tsx` - UI for instruction input, preview display, editing
- **Frontend Service**: `useCaseService.previewUseCaseFromPrompt()` - API call
- **Backend Controller**: `use_case_controller.previewUseCaseFromPrompt`
- **Backend Service**: `UseCasePromptService.previewUseCaseFromPromptAsync()`
- **LLM Agent**: `UseCaseFromPromptAgent` - Defines system prompt and LLM settings

### Test Case vs Use Case Relationship

- **Use Case**: High-level user story with breakdown (requirement + acceptance criteria pairs)
- **Test Case**: Specific test scenario derived from a use case, belongs to a `useCaseId`
- Current flow: Use Case → Auto-generate Test Cases via `TestCaseProposalAgent`

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| Use Case Requirement | Required for MVP | Maintains data consistency, leverages existing context for better generation |
| Default Values (displayOptions, defaultParallelRuns) | Use project defaults | Don't burden LLM with non-essential fields |
| URL Field | Inherit from use case/project | Allow user override in preview |
| Automated Flag | Default to `true` | Indicates script availability for replay; updated when script is generated |
| Batch Support | One prompt → one or more test cases | LLM decides appropriate scope based on instruction complexity |

## Proposed Architecture

### 1. New Interface: `ITestCasePromptInput`

**Location**: `muggle-ai-prompt-service/src/interfaces/muggleTest/testCase/ITestCasePromptInput.ts`

| Field | Type | Description |
|-------|------|-------------|
| `instruction` | `string` | User's natural language prompt describing the test scenario(s) |

### 2. New Interface: `ITestCaseCreationResponse`

**Location**: `muggle-ai-prompt-service/src/interfaces/muggleTest/testCase/ITestCaseCreationResponse.ts`

LLM output structure for a single test case:

| Field | Type | Description |
|-------|------|-------------|
| `title` | `string` | Short phrase describing the test case |
| `description` | `string` | Detailed explanation of the test case |
| `goal` | `string` | Concise, measurable goal of the test |
| `precondition` | `string` | Initial state/setup required before test execution |
| `expectedResult` | `string` | Expected outcome after test execution |
| `tags` | `string[]` | Relevant tags for categorization |
| `category` | `string` | Test case category (e.g., "Smoke", "Regression", "Edge Case") |
| `priority` | `TestCasePriority` | Suggested priority level |

The LLM returns `{ testCases: ITestCaseCreationResponse[] }` to support multiple test cases from one prompt.

### 3. New LLM Agent: `TestCaseFromPromptAgent`

**Location**: `muggle-ai-prompt-service/src/products/muggleTest/agentDefinition/TestCaseFromPromptAgent.ts`

**Purpose**: Generate one or more test cases from a user prompt with project/use case context.

**LLM Settings**:
| Setting | Value |
|---------|-------|
| Model | `gpt-4o-mini` |
| Temperature | `0` |
| Max Tokens | `16384` |
| Response Format | `json_object` |

**System Prompt Design Goals**:
- Role: Senior QA engineer
- Input: Project context, use case context, user instruction
- Output: JSON array of test cases
- Guidelines:
  - Generate 1-5 test cases depending on instruction scope
  - Cover happy path and relevant edge cases
  - Each test case should be focused and testable
  - Include meaningful preconditions and expected results

**Comparison with Existing `TestCaseProposalAgent`**:

| Aspect | TestCaseProposalAgent | TestCaseFromPromptAgent (New) |
|--------|----------------------|-------------------------------|
| Purpose | Generate test cases from use case automatically | Generate test cases from user prompt interactively |
| Input | Use case details only | User prompt + project/use case context |
| Output | Array of proposals (workflow) | Array of test case previews (interactive) |
| Trigger | Automated workflow | User-initiated via UI |
| Usage | Background processing | Real-time preview with editing |

### 4. New Backend Service: `TestCasePromptService`

**Location**: `muggle-ai-prompt-service/src/service/test_case_prompt_service.ts`

**Method**: `previewTestCasesFromPromptAsync`

| Parameter | Type | Description |
|-----------|------|-------------|
| `callerUserId` | `string` | Authenticated user ID |
| `projectId` | `string` | Target project ID |
| `useCaseId` | `string` | Parent use case ID |
| `promptInput` | `ITestCasePromptInput` | User's instruction |

**Returns**: `ITestCaseCreationRequest[]` (array of test case previews, not persisted)

**Flow**:
1. Validate all required parameters
2. Check authorization (`VIEW_PROJECT` permission)
3. Fetch project context (name, URL, description)
4. Fetch use case context (title, userStory, description, breakdown)
5. Call `TestCaseFromPromptAgent` with enriched prompt
6. Parse and validate LLM JSON response
7. Map LLM response to `ITestCaseCreationRequest[]`:
   - Set `projectId` and `useCaseId` from parameters
   - Set `url` from use case (fallback to project URL)
   - Set `status` to `DRAFT`
   - Set `automated` to `true`
   - Inherit `displayOptions` and `defaultParallelRuns` from project defaults (or omit)
8. Return array of preview test cases

### 5. New Backend Controller Endpoint

**Location**: `muggle-ai-prompt-service/src/controllers/muggle_test/test_case_controller.ts`

**Handler**: `previewTestCasesFromPrompt`

**Route**: `POST /muggle-test/projects/:projectId/use-cases/:useCaseId/test-cases/prompt/preview`

**Request Body**:
```json
{
  "instruction": "Test that user can add item to cart and proceed to checkout"
}
```

**Response** (200 OK):
```json
{
  "testCases": [
    {
      "projectId": "proj_123",
      "useCaseId": "uc_456",
      "title": "User adds single item to cart successfully",
      "description": "...",
      "goal": "Verify item appears in cart with correct details",
      "precondition": "User is logged in, product page is loaded",
      "expectedResult": "Cart shows 1 item with correct name, price, and quantity",
      "status": "DRAFT",
      "priority": "HIGH",
      "url": "https://example.com/products",
      "tags": ["cart", "e-commerce", "happy-path"],
      "category": "Functional",
      "automated": true
    },
    {
      "projectId": "proj_123",
      "useCaseId": "uc_456",
      "title": "User proceeds to checkout from cart",
      "description": "...",
      ...
    }
  ]
}
```

### 6. Frontend Service Extension

**Location**: `muggle-prompt-ui/src/services/muggleTest/testCaseService.ts`

**New Method**:

| Method | Parameters | Returns |
|--------|------------|---------|
| `previewTestCasesFromPrompt` | `projectId`, `useCaseId`, `prompt`, `authToken` | `Promise<ICreateTestCaseRequest[]>` |

### 7. New Frontend Type

**Location**: `muggle-prompt-ui/src/types/muggleTest/testCase/ITestCasePromptInput.ts`

Mirrors backend interface for type safety.

### 8. New Frontend Component: `TestCasePromptModal`

**Location**: `muggle-prompt-ui/src/components/products/muggleTestV0/components/TestCasePromptModal.tsx`

**Props**:

| Prop | Type | Description |
|------|------|-------------|
| `open` | `boolean` | Modal visibility state |
| `onOpenChange` | `(open: boolean) => void` | Callback for visibility changes |
| `projectId` | `string` | Current project ID |
| `useCaseId` | `string` | Selected use case ID |
| `useCase` | `IUseCase` (optional) | Use case object for context display |
| `projectName` | `string` (optional) | Project name for context display |
| `projectDescription` | `string` (optional) | Project description for context |
| `onAdd` | `(testCases: ICreateTestCaseRequest[]) => Promise<void>` | Callback to persist test cases |

**Component State**:

| State | Type | Purpose |
|-------|------|---------|
| `instruction` | `string` | User input text |
| `previews` | `ICreateTestCaseRequest[]` | Generated test case previews |
| `isGenerating` | `boolean` | Loading state for generation |
| `isAdding` | `boolean` | Loading state for save operation |
| `selectedIndices` | `Set<number>` | Indices of test cases selected for adding |
| `collapsedItems` | `Record<number, boolean>` | Collapse state for each preview card |

**UI Layout**:

```
┌─────────────────────────────────────────────────────────────────────┐
│ Add Test Cases: Generate from Instruction                     [X]  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  ┌─────────────────────────┐  ┌─────────────────────────────────┐  │
│  │ Instruction             │  │ Test Case Previews              │  │
│  │                         │  │                                 │  │
│  │ Use Case Context:       │  │ [Select All] [Deselect All]     │  │
│  │ ┌─────────────────────┐ │  │ ┌─────────────────────────────┐ │  │
│  │ │ Title: Add to Cart  │ │  │ │ [✓] Test Case 1          ▼ │ │  │
│  │ │ Story: As a user... │ │  │ │     Title: [editable]      │ │  │
│  │ └─────────────────────┘ │  │ │     Goal: [editable]       │ │  │
│  │                         │  │ │     Precondition: [edit]   │ │  │
│  │ ┌─────────────────────┐ │  │ │     Expected: [editable]   │ │  │
│  │ │ Describe what you   │ │  │ │     Priority: [dropdown]   │ │  │
│  │ │ want to test...     │ │  │ │     Tags: [editable]       │ │  │
│  │ │                     │ │  │ └─────────────────────────────┘ │  │
│  │ └─────────────────────┘ │  │ ┌─────────────────────────────┐ │  │
│  │                         │  │ │ [✓] Test Case 2          ▼ │ │  │
│  │ [Generate Preview]      │  │ │     ...                     │ │  │
│  │                         │  │ └─────────────────────────────┘ │  │
│  └─────────────────────────┘  └─────────────────────────────────┘  │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│                              [Cancel]  [Add Selected (2) to Plan]  │
└─────────────────────────────────────────────────────────────────────┘
```

**Key Features**:
- Display use case context to help user write better prompts
- Support selecting/deselecting individual test cases from the generated batch
- Each test case preview is collapsible and editable
- "Add Selected" button shows count and only adds checked test cases

## Integration Points

### Trigger Locations

1. **Test Cases Page** (`testcases/page.tsx`):
   - Add "Generate from Prompt" button in the header actions
   - Requires use case filter/selection to be active
   - Pass selected `useCaseId` to modal

2. **Use Case Detail View** (if exists):
   - Add "Generate Test Cases from Prompt" action
   - Use case context already available

### Router Registration

**Location**: `muggle-ai-prompt-service/src/routers/v1/muggle_test_router.ts`

Add new route:
```
POST /muggle-test/projects/:projectId/use-cases/:useCaseId/test-cases/prompt/preview
→ test_case_controller.previewTestCasesFromPrompt
```

## File Changes Summary

### Backend (muggle-ai-prompt-service)

| File | Change Type | Description |
|------|-------------|-------------|
| `src/interfaces/muggleTest/testCase/ITestCasePromptInput.ts` | New | Prompt input interface |
| `src/interfaces/muggleTest/testCase/ITestCaseCreationResponse.ts` | New | LLM response interface |
| `src/products/muggleTest/agentDefinition/TestCaseFromPromptAgent.ts` | New | LLM agent definition |
| `src/service/test_case_prompt_service.ts` | New | Preview service |
| `src/controllers/muggle_test/test_case_controller.ts` | Modify | Add preview handler |
| `src/routers/v1/muggle_test_router.ts` | Modify | Add route |

### Frontend (muggle-prompt-ui)

| File | Change Type | Description |
|------|-------------|-------------|
| `src/types/muggleTest/testCase/ITestCasePromptInput.ts` | New | Prompt input type |
| `src/services/muggleTest/testCaseService.ts` | Modify | Add preview method |
| `src/components/products/muggleTestV0/components/TestCasePromptModal.tsx` | New | Modal component |
| `src/products/muggleTestV0/dashboard/projects/[id]/testcases/page.tsx` | Modify | Integrate modal |

## Implementation Order

**Phase 1: Backend - Riskless Additions** (no impact to existing flows)
1. Add `ITestCasePromptInput` interface
2. Add `ITestCaseCreationResponse` interface  
3. Add `TestCaseFromPromptAgent` LLM agent definition

**Phase 2: Backend - Service Layer**
4. Add `TestCasePromptService` with `previewTestCasesFromPromptAsync`

**Phase 3: Backend - API Hook-up**
5. Add controller handler `previewTestCasesFromPrompt`
6. Register route in router

**Phase 4: Frontend - Riskless Additions**
7. Add `ITestCasePromptInput` type
8. Add service method `previewTestCasesFromPrompt`
9. Add `TestCasePromptModal` component (standalone, not integrated)

**Phase 5: Frontend - Integration**
10. Integrate modal into test cases page
11. Wire up use case selection flow

## Error Handling

| Error Case | Handling |
|------------|----------|
| Empty instruction | Frontend validation, toast message |
| Project not found | 404 response, toast error |
| Use case not found | 404 response, toast error |
| LLM response parse failure | 500 response with logged context, toast error |
| LLM returns empty array | Return empty array, show "No test cases generated" message |
| Authorization failure | 403 response, toast error |

## Future Enhancements

1. **Standalone Mode**: Allow test case generation without pre-existing use case (auto-create ad-hoc use case)
2. **Regenerate Individual**: Button to regenerate a single test case in the preview list
3. **Template Prompts**: Pre-defined prompt templates for common test scenarios
4. **History**: Save and reuse previous prompts
