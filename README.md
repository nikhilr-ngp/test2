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
###  General flow for any website
``` mermaid 
sequenceDiagram
    participant User
    participant Frontend (Client)
    participant Backend (Server)
    participant Google OAuth

    User->>Frontend (Client): Click "Sign in with Google"
    Frontend (Client)->>Google OAuth: Redirect with client_id, scope, redirect_uri
    Google OAuth->>User: Show consent screen
    User->>Google OAuth: Grant permissions
    Google OAuth->>Frontend (Client): Redirect back with authorization code
    Frontend (Client)->>Backend (Server): Send authorization code
    Backend (Server)->>Google OAuth: Exchange code for access token
    Google OAuth->>Backend (Server): Respond with access & refresh token
    Backend (Server)->>Google OAuth: Fetch user email with access token
    Google OAuth->>Backend (Server): Return user info (Gmail, profile, etc.)
    Backend (Server)->>Database: Store user details
    Backend (Server)->>Frontend (Client): Log in the user & start session
```

### General Flow for OpenAi website
``` mermaid
sequenceDiagram
    participant User (You)
    participant OpenAI Website (Frontend)
    participant OpenAI Server (Backend)
    participant Google OAuth Server

    User (You)->>OpenAI Website (Frontend): Click "Sign in with Google"
    OpenAI Website (Frontend)->>Google OAuth Server: Redirect with client_id, scope, redirect_uri
    Google OAuth Server->>User (You): Show consent screen
    User (You)->>Google OAuth Server: Grant permissions
    Google OAuth Server->>OpenAI Website (Frontend): Redirect back with authorization code
    OpenAI Website (Frontend)->>OpenAI Server (Backend): Send authorization code
    OpenAI Server (Backend)->>Google OAuth Server: Exchange code for access token
    Google OAuth Server->>OpenAI Server (Backend): Respond with access token & refresh token
    OpenAI Server (Backend)->>Google OAuth Server: Fetch user email with access token
    Google OAuth Server->>OpenAI Server (Backend): Return Gmail & profile info
    OpenAI Server (Backend)->>Database: Store user details (Gmail, tokens if needed)
    OpenAI Server (Backend)->>OpenAI Website (Frontend): Log in the user & start session
```





### PMS flow

``` mermaid
sequenceDiagram
    participant User
    participant Frontend (Client)
    participant Backend (AdonisJS Server)
    participant Google OAuth Server
    participant Database

    User->>Frontend (Client): Click "Sign in with Google"
    Frontend (Client)->>Google OAuth Server: Redirect for authentication
    Google OAuth Server->>Frontend (Client): Returns ID Token & Access Token
    Frontend (Client)->>Backend (AdonisJS Server): Sends tokens for verification
    Backend (AdonisJS Server)->>Google OAuth Server: Verifies ID Token
    Google OAuth Server->>Backend (AdonisJS Server): Returns verification response
    Backend (AdonisJS Server)->>Database: Checks if user exists
    Database->>Backend (AdonisJS Server): Returns employee details
    Backend (AdonisJS Server)->>Database: Updates user profile (if necessary)
    Backend (AdonisJS Server)->>User: Returns JWT token for authentication
```
