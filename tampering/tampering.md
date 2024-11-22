# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

   `npm install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

   ```
       <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
   ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

   a. Setting httpOnly to false allows client-side scripts to access cookies, making them susceptible to XSS attacks. sameSite is not set, which increases the risk of CSRF attacks.

   b. User inputs (example - name submitted via /register) are not sanitized, making the application vulnerable to XSS attacks.

   c. If the secret is hardcoded or weak, attackers can guess it, potentially allowing them to forge valid session cookies.

2. Briefly explain how a malicious attacker can exploit them.

   a. Attackers can use JavaScript in the browser to steal session cookies and hijack user sessions. Without sameSite protection, an attacker can trick a user into visiting a malicious site that makes authenticated requests to the target app.

   b. Attackers can inject malicious scripts in the name field when submitting via /register, leading to XSS attacks.

   c. Predictable or weak session secrets allow attackers to generate valid session cookies, leading to unauthorized access.

3. Briefly explain why **secure.ts** does not have the same vulnerabilties?

   a. Setting httpOnly to true prevents client-side scripts from accessing session cookies, mitigating XSS risks.

   b. Setting sameSite to strict protects against CSRF attacks by ensuring cookies are only sent for same-site requests.

   c. The escapeHTML function sanitizes user inputs, escaping HTML special characters and preventing XSS attacks.

   d. The secret is passed as a runtime argument, making it harder for attackers to guess or reuse.
