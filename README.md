``` mermaid 

sequenceDiagram
    participant User
    participant Client (Browser)
    participant Server (Backend)
    participant Google OAuth Server

    User->>Client: Click "Sign In with Google"
    Client->>Google OAuth Server: Redirect with client_id, scope, redirect_uri, state, response_type=code
    Google OAuth Server->>User: Display consent screen
    User->>Google OAuth Server: Grants permission
    Google OAuth Server->>Client: Redirect back with authorization code & state
    Client->>Server: Send authorization code
    Server->>Google OAuth Server: POST request (code, client_id, client_secret, redirect_uri, grant_type)
    Google OAuth Server->>Server: Respond with access token (and refresh token)
    Server->>Google API: GET user info (with access token)
    Google API->>Server: Respond with user’s Gmail and profile info
    Server->>Database: Store user Gmail & tokens
    Server->>Client: Create session & log user in
```
