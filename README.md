# Authentication
----------------------------------------------------------------------------------------------------------------------------------------------------------------------

Authentication is essential for any website because certain content must be protected. Not all users or visitors should be able to access all parts of a website.
That’s why authentication exists — to restrict access to specific areas or data.
Protected content can vary — sometimes, we have pages or resources that should only be accessible to authenticated users.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Authentication is a Two-Step Process

(1) Get Access / Permission.
(2) Send Request to Protected Resource.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

             ->   Request (with user credentials) ->
Client (Browser)                                     Server   
             <-        Response {Yes / No}          <-

Is that enough?
A simple “ Yes ” is not enough to access protected resources (API endpoints).

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------

We have our client (the React app running in the browser) and a backend server containing server-side code — either written by us or by someone else.
It could be a third-party API, an external API we are integrating with, or our own API.
The process starts when we send a request containing an email-password combination to the server.
The server then verifies these credentials and either grants or denies permission for further requests. Once permission is granted, we can use it to access protected resources.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Disadvantage
We can’t simply save and reuse the “Yes” response.
A malicious client could fake a “Yes” response to request protected data.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Two Main Approaches for Authentication

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

(1) Server-Side Sessions
(2) Authentication Tokens
We can handle authentication using either server-side sessions or authentication tokens.

------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Server-Side Sessions


Server-side sessions are a traditional and reliable approach to authentication.
Once a server grants access, it stores a unique identifier for that client.
This means that for every authenticated user, the server generates and stores a specific identifier.
The same identifier is also sent back to the client.
So, instead of simply responding with “Yes” or “No,” the server now includes this unique identifier in the response.
Both the server and client share this identifier, and it is attached to all future requests from that client.

Server stores unique identifier → sends it to client.  
Client attaches the identifier → with each future request.

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Disadvantage

This approach works well when the frontend and backend are tightly coupled, such as a traditional web app served entirely by a single server.
However, in modern architectures — for example, when your frontend (React SPA) is hosted on Server A and your backend API runs on Server B — there isn’t such tight coupling.
In these cases, the backend should remain stateless, meaning it shouldn’t store client-specific session data.
For APIs that serve multiple frontends (e.g., Google Maps API), maintaining session data for each client is not feasible.
That’s why we use authentication tokens instead.

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Authentication Tokens

The concept here is similar, but with one key difference.
You still send credentials (email and password) to the server.
The server validates them by comparing the provided credentials with what’s stored in the database.
If the credentials are valid, the server generates a permission token — a long encoded string containing user data (e.g., email address and other details).
This token is created using a specific algorithm and a private key known only to the server.
The token is not stored on the server; instead, it is sent back to the client.
The client (e.g., the React app) then attaches this token to all future requests to access protected resources.
Even though the server does not store this token, it can verify its authenticity using the same private key that was used during creation.
If someone tries to forge a token with a different key, the server will detect it and deny access.
This approach allows the server to remain stateless while still securely identifying and authorizing clients.# Authentication Tokens
Server creates (but does not store) a “permission” token and sends it to the client.  
Client attaches this token to all future requests to protected resources.

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
| Approach                  | Description                                | Suitable For                                    |
| ------------------------- | ------------------------------------------ | ----------------------------------------------- |
| **Server-Side Sessions**  | Stores client identifiers on the server.   | Tightly coupled apps (same frontend & backend). |
| **Authentication Tokens** | Server issues a signed token (not stored). | Stateless, decoupled systems (APIs, SPAs).      |


