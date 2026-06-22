# Login & Account Security – Test Cases

## Scenarios Covered

1. Correct login
2. Wrong password
3. Empty fields
4. Locked user
5. Password reset
6. Session timeout
7. Logout and browser-back behaviour

## Test Cases (15)

| ID | Title | Precondition | Test Data | Steps | Expected Result | Type |
|----|-------|-------------|-----------|-------|----------------|------|
| AUTH-001 | Correct login with valid credentials | Registered active user exists | Email: user@test.de, PW: ValidP@ss1 | 1. Open login page 2. Enter email 3. Enter password 4. Click Login | Redirect to dashboard, session cookie set, welcome message | Positive |
| AUTH-002 | Login with wrong password | Registered user exists | Email: user@test.de, PW: WrongP@ss1 | 1. Enter email 2. Enter wrong password 3. Submit | "Invalid email or password", no session cookie, no hint which field is wrong | Negative |
| AUTH-003 | Login with empty email field | Login page loaded | Email: (empty), PW: (any) | 1. Leave email empty 2. Enter arbitrary password 3. Submit | Client-side validation: "Email is required", form not submitted | Negative |
| AUTH-004 | Login with empty password field | Login page loaded | Email: user@test.de, PW: (empty) | 1. Enter valid email 2. Leave password empty 3. Submit | Client-side validation: "Password is required", form not submitted | Negative |
| AUTH-005 | Login with both fields empty | Login page loaded | Email: (empty), PW: (empty) | 1. Submit with empty fields | "Email is required" and "Password is required" shown simultaneously | Negative |
| AUTH-006 | Login with locked/disabled user | User status = locked (5 failed attempts) | Email: locked@test.de, PW: correct password | 1. Enter correct credentials 2. Submit | "Account is locked. Please try again in 15 minutes." or contact support | Negative |
| AUTH-007 | Account auto-lock after 5 failed attempts | User with 4 failed logins | Email: user@test.de | 1. Enter wrong password 5th time | After 5th failure: "Account temporarily locked (15 min)", no login possible | Security |
| AUTH-008 | Password reset – valid email | Registered user | Email: user@test.de | 1. Click "Forgot password" 2. Enter email 3. Submit 4. Check email 5. Click reset link 6. Set new password | Reset email sent within 30s, link valid 1h, new password works | Positive |
| AUTH-009 | Password reset – invalid/non-existent email | No account exists | Email: nonexistent@test.de | 1. Click "Forgot password" 2. Enter unknown email 3. Submit | "If an account exists, a reset email has been sent." (no info leakage) | Negative |
| AUTH-010 | Password reset – expired token | Reset token generated, waited >1h | Expired token | 1. Click expired reset link 2. Submit new password | "Reset link expired. Please request a new one." | Negative |
| AUTH-011 | Session timeout after inactivity | User logged in, idle | Session TTL: 30 min | 1. Log in 2. Wait 31 min 3. Click any navigation link | Redirect to login page, "Your session has expired" message | Positive |
| AUTH-012 | Session timeout – API token also invalidated | Same session as AUTH-011 | Expired session | 1. After timeout, call protected API with old session token | 401 Unauthorized, "Invalid or expired session" | Security |
| AUTH-013 | Logout destroys session | Logged-in user on dashboard | Valid session | 1. Click Logout 2. Try to use browser "Back" button 3. Try to navigate to dashboard URL | Logged out → redirected to login page. Back button shows login, not dashboard. Private data not cached | Security |
| AUTH-014 | Browser back after logout shows login, not cached page | User AUTH-013 completed, session destroyed | Same browser tab | 1. After logout 2. Press browser back 3. Press browser forward | Back shows login page (not cached dashboard). Forward shows login page. No-cache headers sent | Security |
| AUTH-015 | Login with email containing leading/trailing spaces | User registered as user@test.de | " user@test.de " with spaces | 1. Enter " user@test.de " 2. Enter correct password | Login successful, spaces trimmed, session created | Edge case |

## Possible Defects

| ID | Title | Steps | Actual | Expected | Severity | Root Cause |
|----|-------|-------|--------|----------|----------|-----------|
| BUG-AUTH-001 | Browser back shows cached dashboard after logout | 1. Login 2. Logout 3. Press browser back | Dashboard visible with cached data | Login page only | **High** | Missing no-cache, no-store headers on protected pages |
| BUG-AUTH-002 | Password reset link accepts expired token without validation | 1. Request reset 2. Wait 2h 3. Click old link 4. Set new password | Password changed successfully with expired token | "Link expired" error | **Critical** | Server does not validate token expiry timestamp |
| BUG-AUTH-003 | Locked user counter resets on successful login before lock | 1. Enter wrong PW 4x 2. Enter correct PW 5th try 3. Logout 4. Enter wrong PW again | Only 1 failure counted, not starting from 5th | Should start from 5th (no new attempts after successful login) | **Medium** | Failure counter resets to 0 on success instead of starting at lock threshold |
