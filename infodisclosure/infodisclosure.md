# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?username[$ne]=
   ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts**

   a. The implementation is vulnerable to NoSQL Injection because it directly uses user-provided input (username) in the query to User.findOne() without any validation or sanitization. This allows an attacker to inject malicious query syntax.

2. Briefly explain how a malicious attacker can exploit them.

   An attacker can exploit the NoSQL injection vulnerability by crafting a malicious query to manipulate the database.

   a. An attacker could send a query like "?username[$ne]=". This modifies the query to always return a user, bypassing any authentication logic.

   b. Using payloads like "?username[$regex]=.\*", an attacker could retrieve matching user records.

   c. Crafting complex or malformed queries can overload the database server, potentially causing performance degradation or crashing the system.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the information disclosure vulnerability?

   a. Input validation ensures the username query parameter is a string using typeof username !== 'string'. This prevents unexpected input types that could be exploited.

   b. Input sanitization uses a sanitization step to remove non-alphanumeric characters, preventing special characters that could manipulate the query.

   c. The implementation wraps the database query in a try-catch block to handle unexpected errors without exposing sensitive details about the database.
