``` mermaid
graph TD;

    %% User Authentication
    P1((User Authentication)) -->|Enter Details| P1.1((Register User))
    P1.1 -->|Save User| DB1[(User Database)]
    P1((User Authentication)) -->|Enter Credentials| P1.2((Login Verification))
    P1.2 -->|Check Details| DB1
    P1.2 -->|Success/Failure| User[User]

    %% User Interaction
    P2((User Interaction)) -->|Request Material| P2.1((View Material))
    P2.1 -->|Fetch from DB| DB2[(Material, Exercise, Content Database)]
    P2((User Interaction)) -->|Request Exercise| P2.2((View Exercises))
    P2.2 -->|Fetch from DB| DB2
    P2((User Interaction)) -->|Request Media| P2.3((View Content))
    P2.3 -->|Fetch from DB| DB2

    %% User Management
    P3((User Management)) -->|Modify Users| P3.1((Update User Info))
    P3.1 -->|Update Data| DB1
    P3((User Management)) -->|View Users| P3.2((Retrieve User List))
    P3.2 -->|Fetch from DB| DB1

    %% Material Management
    P4((Material Management)) -->|Add Material| P4.1((Save Material))
    P4.1 -->|Store in DB| DB2
    P4((Material Management)) -->|View Material| P4.2((Retrieve Material))
    P4.2 -->|Fetch from DB| DB2

    %% Exercise Management
    P5((Exercise Management)) -->|Add Exercise| P5.1((Save Exercise))
    P5.1 -->|Store in DB| DB2
    P5((Exercise Management)) -->|View Exercise| P5.2((Retrieve Exercise))
    P5.2 -->|Fetch from DB| DB2

    %% Content Management
    P6((Content Management)) -->|Add Media| P6.1((Save Media))
    P6.1 -->|Store in DB| DB2
    P6((Content Management)) -->|View Media| P6.2((Retrieve Media))
    P6.2 -->|Fetch from DB| DB2

```

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

### Node JS
``` mermaid
graph TD;
    
    A[Client Requests] -->|Incoming Requests| B[Single Threaded Event Loop];
    
    B -->|I/O Operations File, Network, DB| C[Delegates to OS Thread Pool];
    
    C -->|Asynchronous Execution| D[Event Queue];
    
    D -->|Callback Registered| E[Callback Function];
    
    E -->|Processed by Event Loop| F[Execution in Main Thread];
    
    F -->|Response Sent| G[Client Receives Response];

    B -->|Microtask Queue Promises, process.nextTick| H[Higher Priority Execution];
    
    H -->|Executed Before Next Event Loop Iteration| F;
    
    style B fill:#f4a261,stroke:#000,stroke-width:2px;
    style D fill:#2a9d8f,stroke:#000,stroke-width:2px;
    style H fill:#e76f51,stroke:#000,stroke-width:2px;

```

### adonis
``` mermaid
graph TD;
    A[Incoming HTTP Request] --> B[Global Middleware];
    B --> C[Route Middleware];
    C --> D[Validator];
    D --> E[Controller];
    E --> F[Service/Repository];
    F --> G[Database or External API];
    G --> H[Return Response to Controller];
    H --> I[Controller Formats Response];
    I --> J[Response Sent to Client];

    style B fill:#f4a261,stroke:#000,stroke-width:2px;
    style C fill:#e76f51,stroke:#000,stroke-width:2px;
    style D fill:#2a9d8f,stroke:#000,stroke-width:2px;
    style E fill:#457b9d,stroke:#000,stroke-width:2px;
    style F fill:#1d3557,stroke:#fff,stroke-width:2px;
    style G fill:#264653,stroke:#fff,stroke-width:2px;
    style J fill:#2a9d8f,stroke:#000,stroke-width:2px;

```
