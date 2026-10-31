# OTP Verification Bypass via HTTP Method Manipulation

**Severity:** Medium

**Status:** Triggered (Appreciation letter received)

**Program:** wscube****.com

**Reporter:** Abishek Khatri

## Summary
A contact form on a web application required OTP verification during its
normal workflow. However, the server-side check was not applied consistently.
By changing the HTTP method of the submission request, the OTP step could be
skipped and the form was still accepted.

## Affected Component
Contact and lead generation forms (OTP verification flow)

## Steps to Reproduce
1. Open a contact form and fill in all required fields.
2. Click Submit and observe the normal OTP flow.
3. Intercept the form submission request using Burp Suite.
4. Change the HTTP method to a different method.
5. Forward the request.
6. Observe that the submission is accepted without completing OTP verification.

## Expected Result
OTP verification should be enforced on the server side, regardless of the
HTTP method used.

## Actual Result
The request was processed successfully without any OTP validation.

## Impact
- Bypass of the intended verification control
- Submission of unverified leads
- Increased spam and abuse risk
- Bulk automated form submissions

## Remediation
1. Enforce OTP verification fully on the server side.
2. Apply the same validation logic to all endpoints and HTTP methods.
3. Reject unsupported HTTP methods with `405 Method Not Allowed`.
4. Log and monitor abnormal submission patterns.

## Acknowledgement
The company acknowledged this finding and issued an appreciation letter.


