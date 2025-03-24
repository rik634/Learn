# JWT Authentication

- JWTs are everywhere in modern authentication, but how do they actually work? 

What’s Inside a JWT?
- A JWT is just a string of three parts:

1. Header (Metadata)
   - Specifies the token type (typ) and algorithm (alg).
      Example: { "alg": "HS256", "typ": "JWT" }

2. Payload (Claims & User Data)
   - Contains user-related information (claims).
   - Types of claims:
     • Public – Can be openly shared.
     • Registered – Standard claims like sub (subject).
     • Private – Custom claims for specific use cases.
     Example:
         { "sub": "12345", "name": "John Doe", "admin": true, "iat": 1716230911 }

3. Signature (Security Layer)
   - Ensures the token hasn’t been modified.
   - Created by signing the header & payload with a secret key.
   - Formula:
         HMACSHA256(base64UrlEncode(header) + "." + base64UrlEncode(payload), secret)

## How JWT Authentication Works ?
1. User Login – The client sends credentials (email & password) to the authentication server.
2. Token Issuance – If valid, the server generates a JWT and sends it to the client.
3. Access Requests – The client includes the JWT in API requests (usually in the Authorization header).
4. Verification & Data Access – The server verifies the JWT and, if valid, grants access.

## Why Use JWT's?
 - Stateless authentication (no need to store sessions).
 - Faster authentication since no database lookup is required.
 - Secure data exchange when implemented correctly.

JWTs are powerful, but they’re not magic! Use them wisely, and never store sensitive data inside a JWT.
