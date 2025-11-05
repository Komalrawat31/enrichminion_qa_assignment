# 🐞 Bug Report

## Sheet: Bug Report

| Bug ID   | Title                                       | Severity   | Steps to Reproduce   | Expected Result                                                    | Actual Result                                                                                                       | Evidence (Screenshots/Logs)   | Root Cause (Frontend/Backend/Both)   |
|----------|---------------------------------------------|------------|----------------------|--------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------|-------------------------------|--------------------------------------|
| BUG-001  | Mouse Pointer Appears Twice                 | Medium     | 1. Open the website
2. Hover the mouse over any clickable area
3. Observe the cursor display                      | Mouse pointer should appear only once.                             | Mouse pointer appears twice on the screen.                                                                          | screenshot_1.png              | Frontend                             |
| BUG-002  | Logo Changes on Page Navigation             | Low        | 1. Open homepage
2. Navigate to another page (e.g., Verification)
3. Observe the logo                      | Logo should remain consistent across all pages.                    | Logo changes when navigating between pages.                                                                         | screenshot_2.png              | Frontend                             |
| BUG-003  | Undefined Text in User Space                | Medium     | 1. Login as user Komal
2. Observe the user space text during page load                      | User space name should remain static (Komal’s Space).              | Displays 'undefined space' for a few seconds during load.                                                           | screenshot_3.png              | Frontend                             |
| BUG-004  | Missing Delete Column in Update History     | Low        | 1. Go to Update History section
2. Look for delete option                      | Delete column should be available for data management.             | No delete column is present in update history table.                                                                | screenshot_4.png              | Frontend                             |
| BUG-005  | User Info Missing on Profile Icon Click     | Medium     | 1. Click on the username profile icon on homepage
2. Observe displayed info                      | User details should be shown upon clicking profile icon.           | No user information displayed after clicking username icon.                                                         | screenshot_5.png              | Frontend                             |
| BUG-006  | Google Sign-in Fails (Provider Not Enabled) | High       | 1. Click on 'Sign in with Google'
2. Observe the response                      | User should be signed in successfully using Google authentication. | Displays error: {"code":400,"error_code":"validation_failed","msg":"Unsupported provider: provider is not enabled"} | error_log.png                 | Backend                              |

