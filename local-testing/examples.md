# Local Testing Examples

Common testing scenarios and example prompts for Muggle Test Local.

## Project-Based Testing Workflow

The recommended approach for comprehensive testing is to organize your tests into projects, use cases, and test cases.

### Step 1: Create a Project

> "Create a local project called 'E-commerce App' for localhost:3000 with description 'Testing our shopping cart features'"

### Step 2: Define Use Cases

> "Add a use case to my E-commerce App project called 'Checkout Flow' with the user story: As a customer, I want to complete my purchase so that I can receive my items"

### Step 3: Add Test Cases

> "Create test cases for the Checkout Flow use case:
> 1. 'Add item to cart' - Add a product and verify cart updates
> 2. 'Apply discount code' - Enter a valid code and verify discount
> 3. 'Complete purchase' - Fill shipping/payment and submit order"

### Step 4: Generate Test Scripts

> "Generate a test script for the 'Add item to cart' test case"

The assistant will launch Muggle AI Studio to record the interaction automatically.

### Step 5: Review Results

> "Show me the run results for my E-commerce App project"

### Step 6: Replay Tests

> "Replay the 'Add item to cart' test script"

### Step 7: Publish to Cloud (Optional)

> "Publish the E-commerce App project to the cloud with production URL https://shop.example.com"

---

## Quick Start Examples

### Explore a Page

Before writing detailed tests, explore what's available:

> "Explore the page at localhost:3000 and tell me what I can interact with"

The assistant will navigate to your app and return a structured analysis of buttons, links, forms, and other interactive elements.

### Run a Simple Test

Describe what you want to test in natural language:

> "Test the login form on localhost:3000/login. Try logging in with email 'test@example.com' and password 'password123'. Verify it redirects to the dashboard."

### Take a Screenshot

Capture the current state:

> "Take a screenshot of the current page"

---

## Form Testing

### Validation Testing

> "Test the signup form at localhost:3000/signup. Try submitting with:
> 1. Empty fields - verify error messages appear
> 2. Invalid email format - verify email validation error
> 3. Password too short - verify password requirements shown
> 4. Valid data - verify success"

### Required Fields

> "On the contact form at localhost:3000/contact, check which fields are required by trying to submit with each field empty one at a time"

### Input Limits

> "Test the bio field on localhost:3000/profile. Try entering 500 characters and see if there's a character limit"

---

## Authentication Flows

### Login Success

> "Test successful login on localhost:3000:
> 1. Navigate to the login page
> 2. Enter email 'user@example.com' and password 'SecurePass123!'
> 3. Click the login button
> 4. Verify redirection to dashboard
> 5. Verify user name appears in the header"

### Login Failure

> "Test login error handling on localhost:3000/login. Enter wrong credentials and verify an appropriate error message is displayed"

### Password Reset

> "Test the forgot password flow starting from localhost:3000/login. Click 'Forgot Password', enter an email, and verify the confirmation message"

### Session Persistence

> "After logging into localhost:3000, refresh the page and verify the user stays logged in"

---

## Navigation Testing

### Menu Links

> "Test the main navigation on localhost:3000. Click each menu item and verify the correct page loads"

### Breadcrumbs

> "Navigate to a deep page on localhost:3000/products/category/item and test that the breadcrumb links work correctly"

### Browser Back Button

> "Navigate through localhost:3000 from home → products → product detail, then test that browser back navigation works correctly"

---

## E-commerce Scenarios

### Product Search

> "Test product search on localhost:3000:
> 1. Search for 'blue shirt' and verify results appear
> 2. Search for 'xyznonexistent' and verify 'no results' message"

### Add to Cart

> "On localhost:3000/products, find a product, click 'Add to Cart', and verify:
> 1. Success message appears
> 2. Cart count updates
> 3. Product appears in cart"

### Cart Operations

> "Test the shopping cart at localhost:3000/cart:
> 1. Change quantity of an item
> 2. Verify subtotal updates
> 3. Remove an item
> 4. Verify it disappears and total updates"

### Checkout Flow

> "Walk through the checkout flow on localhost:3000 starting from a cart with items. Document each step and any validation that occurs"

---

## Error Handling

### 404 Pages

> "Navigate to localhost:3000/page-that-does-not-exist and verify there's a proper 404 page with a way to get back to the main site"

### Form Submission Errors

> "Test what happens on localhost:3000/contact when submitting a form and the server returns an error. Look for appropriate error handling"

### Network Issues

> "On localhost:3000, test how the app handles slow loading by observing any loading states or spinners"

---

## Accessibility Checks

### Keyboard Navigation

> "Test keyboard navigation on localhost:3000. Tab through the page and check:
> 1. Is there a visible focus indicator?
> 2. Is the tab order logical?
> 3. Can you activate buttons with Enter?"

### Form Labels

> "Check the forms on localhost:3000 to verify all input fields have associated labels"

### Image Alt Text

> "Explore localhost:3000 and report on whether images have appropriate alt text"

---

## Step-by-Step Control

For precise control, guide the assistant through individual actions:

### Sequential Actions

> "On localhost:3000, do the following step by step:
> 1. Click the 'Sign Up' button
> 2. Wait 2 seconds for the modal to load
> 3. Type 'newuser@test.com' in the email field
> 4. Type 'SecurePass123!' in the password field
> 5. Click 'Create Account'
> 6. Take a screenshot of the result"

### Conditional Testing

> "On localhost:3000/dashboard:
> 1. Check if there's a notification badge
> 2. If yes, click it and describe what appears
> 3. If no, describe what's in the header area"

---

## Session Management

### Review Past Tests

> "Show me my recent testing sessions"

### Find Specific Results

> "List testing sessions from today"

### Clean Up Old Data

> "Delete testing sessions older than 7 days"

---

## Tips for Effective Testing

### Be Specific

Instead of vague instructions:

| Less Effective | More Effective |
| :------------- | :------------- |
| "Test the form" | "Test the contact form on /contact by submitting with valid data" |
| "Check the button" | "Click the blue 'Submit' button in the login form" |
| "See if it works" | "Verify a success message appears after form submission" |

### Include Expected Outcomes

Tell the assistant what success looks like:

> "After clicking 'Add to Cart', verify:
> - A confirmation toast appears
> - The cart icon shows '1'
> - The button changes to 'Added'"

### Use Exploration First

Before detailed testing, understand the page:

> "First explore localhost:3000/checkout to see what elements are available, then we'll test them"

### Break Down Complex Flows

For multi-step processes, test in stages:

> "Let's test checkout in parts:
> 1. First, test the shipping address form
> 2. Then, test the payment form
> 3. Finally, test the order confirmation"

### Review Results

After complex tests, check what happened:

> "Show me the test results from that last session"

---

## Example Conversation

Here's a complete testing conversation:

---

**You**: I just built a new contact form at localhost:3000/contact. Can you help me test it?

**Assistant**: I'll help you test the contact form. Let me first explore the page to understand its structure.

*[Explores localhost:3000/contact]*

The contact form has:
- Name field (required)
- Email field (required)
- Subject dropdown (optional)
- Message textarea (required)
- Submit button

What would you like to test first?

---

**You**: Test the validation - try submitting with empty fields

**Assistant**: I'll test form validation by submitting empty fields.

*[Runs test]*

Results:
1. Clicked Submit with all fields empty
2. Validation errors appeared:
   - "Name is required" ✓
   - "Email is required" ✓
   - "Message is required" ✓
3. Form was not submitted ✓

Screenshot saved. The validation is working correctly for required fields.

---

**You**: Now test with an invalid email

**Assistant**: Testing email validation.

*[Executes actions]*

1. Entered "John" in name field
2. Entered "not-an-email" in email field
3. Entered "Test message" in message
4. Clicked Submit

Result: Error message "Please enter a valid email address" appeared ✓

Email validation is working correctly.

---

**You**: Great, now test a successful submission

**Assistant**: Testing successful form submission.

*[Runs test]*

1. Filled all fields with valid data:
   - Name: "Test User"
   - Email: "test@example.com"
   - Subject: "General Inquiry"
   - Message: "This is a test message"
2. Clicked Submit
3. Result: Success message "Thank you! We'll be in touch soon." appeared ✓

Form submission is working correctly. All tests passed!

---

## Using Agent Skills

Instead of manually following these workflows, you can install **Agent Skills** that automate common patterns.

### "Test my changes"

With the `test-feature-local` skill installed, simply say:

> "Test my changes"

The agent will:
1. Analyze your `git diff` to identify impacted features
2. Find matching test cases in your local projects
3. Run affected tests and report results

### "Publish test to cloud"

With the `publish-test-to-cloud` skill installed:

> "Publish my test run to the cloud"

The agent handles authentication, action script upload, and linking to the test case automatically.

See **[Agent Skills](skills.md)** for installation instructions.

---

## Next Steps

- **[Agent Skills](skills.md)** — Install pre-built testing workflows
- **[Tools Reference](tools-reference.md)** — Detailed tool documentation
- **[Local Testing Setup](setup.md)** — Installation guide
- **[Common Issues](../troubleshooting/common-issues.md)** — Troubleshooting help
