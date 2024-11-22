# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

   `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

   `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called **infodisclosure**. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

   `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

   ```
       http://localhost:3000/userinfo?id[$ne]=
   ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. Briefly explain the potential vulnerabilities in **insecure.ts** that can lead to a DoS attack.

   a. The application does not impose any limits on the frequency or volume of requests from a single client or IP address.

   b. The User.findOne({ \_id: uid }).exec() operation queries the database for every incoming request. A high volume of such requests can overwhelm the database server, leading to performance degradation or a complete crash.

2. Briefly explain how a malicious attacker can exploit them.

   The attacker can exploit this vulnerability by sending a large number of requests in a short period. This can overload the server, overwhelm the database and make the service unavailable.

3. Briefly explain the defensive techniques used in **secure.ts** to prevent the DoS vulnerability?

   a. The implementation configures the express-rate-limit middleware to restrict the number of requests from a single IP address to 1 request per 5 seconds. This reduces the likelihood of DoS attacks by limiting the attacker's ability to flood the server with requests.

   b. The implementation provides a clear error message to clients who exceed the rate limit, maintaining user experience.
