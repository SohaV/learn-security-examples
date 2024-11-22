# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

   `$ npx install`

2. Start the **insecure.ts** server

   `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

   `npx ts-node mal.ts`

4. Open **http://localhost:8000** in a browser, type a name and Submit.

5. Open the **Application** tab in the Browser's inspect pane. Find the **Cookies** under **Storage**. You should see a **connect.sid** cookie being set.

6. Open the HTML file **mal-steal-cookie.html** file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file **mal-csrf.html** file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out?

## For you to answer

1. Briefly explain the spoofing vulnerability in **insecure.ts**.

   a. The session cookie (httpOnly: false) can be accessed and modified by client-side JavaScript, making it easier for attackers to steal or manipulate the session cookie using cross-site scripting attacks.

   b. The session encryption key (SOMESECRET) is hardcoded, making it predictable. An attacker who gains access to the source code can decode sessio cookies and tamper with them.

   c. Missing sameSite attribute - The cookie is vulnerable to CSRF attacks, where malicious websites can make unauthorized requests on behalf of the user by exploiting the browser's automatic sending of cookies.

2. Briefly explain different ways in which vulnerability can be exploited.

   a. Session Hijacking - An attacker uses a malicious script to read and steal the session cookie from the browser. The attacker can then use the stolen cookie to impersonate the legitimate user.

   b. Cross-Site Request Forgery (CSRF) - An attacker tricks the user into making a malicious request (Example - clicking a link) to the vulnerable site. The browser automatically sends the user's session cookie, allowing unauthorized actions to be performed.

3. Briefly explain why **secure.ts** does not have the spoofing vulnerability in **insecure.ts**.

   a. Since httpOnly is set to true, the session cookie cannot be accessed or modified via client-side JavaScript, preventing XSS attacks from stealing session cookies.

   b. Since sameSite is set to true, the cookie is restricted from being sent with cross-site requests, mitigating CSRF attacks.

   c. The session secret is passed as an argument when starting the server, ensuring it is not hardcoded. This prevents attackers from easily guessing the secret.
