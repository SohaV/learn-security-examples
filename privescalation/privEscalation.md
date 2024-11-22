# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

   ```
       http://localhost:3000/send-form
   ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

   a. The application assumes that the userId provided in the request is valid and belongs to the authenticated user. There is no proper session management to authenticate and verify the identity of the user.

   b. Authorization is based only on the role of the user found in the simulated database, which is insecure since any user can impersonate another by providing a valid userId in the request body.

   c. Non-admin users can potentially escalate their privileges by modifying the newRole parameter in the request body if the role check is bypassed.

2. Briefly explain how a malicious attacker can exploit them.

   a. By sending a POST request to /update-role with a valid userId and newRole, the attacker can update their role to admin or any other privileged role without being properly authenticated or authorized.

   b. An attacker who knows or guesses the userId of other users can modify their roles, leading to unauthorized access and potentially compromising the system.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the privilege escalation vulnerability?

   a. The express-session middleware is used to manage user sessions. Each user is assigned a unique session tied to their userId, ensuring that requests are made by authenticated users only. If req.session.userId is missing, the request is rejected with a 401 Unauthorized status.

   b. The logged-in user's role is checked against the admin requirement before allowing sensitive actions such as updating another user's role. This ensures that only users with administrative privileges can perform such operations.

   c. The implementation checks if the userId exists in the database and verifies it belongs to the logged-in user before performing the role update.
