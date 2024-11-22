# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Run the server **insecure.ts**.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. Briefly explain the vulnerability.

   a. There is no mechanism to authenticate users before allowing them to send or retrieve messages. Any user or attacker can access the /get-messages and /send-message endpoints without verification.

   b. All messages can be retrieved by any client, including unauthorized users, exposing sensitive user data.

2. Briefly explain why the vulnerability is addressed in **secure.ts**.

   a. secure.ts implements an authentication check (e.g., isAuthenticated) to ensure that only authorized users can send and retrieve messages. Unauthorized requests receive a 401 Unauthorized response.

   b. The middleware logs all incoming requests, along with timestamps and IP addresses, to a file (server.log). This creates an audit trail, allowing administrators to detect and investigate unauthorized or suspicious activity.

   c. Errors are logged systematically with timestamps, and the user receives a standardized error response. This improves security and reliability by preventing sensitive stack traces from being exposed to the client.

   d. The secure version ensures that sensitive operations (e.g., retrieving messages) are tied to an authenticated user.

3. Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works?

   The Middleware Design Pattern is used to address the vulnerabilities. Middleware functions in Express.js are layered in the request-response cycle to handle specific concerns such as authentication, logging, and error handling.

   In secure.ts, middleware handles logging, authentication and error handling.

   Logging - It captures and logs details about each request.

   Authentication - Simulates user validation and blocks unauthorized access.

   Error Handling - Catches errors thrown in the application and sends a user-friendly response while logging details for debugging.

   This modular approach separates concerns and improves code readability, maintainability, and security.
