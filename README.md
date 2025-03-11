```mermaid
graph LR
    A[User Query] --> B{Query Embedding};
    B --> C[Embedding Model];
    C --> D[Query Vector];
    D --> E{Vector Search};
    E --> F[Vector Database/Index];
    F --> G[Top K Relevant Documents];
    G --> H{Contextualization};
    H --> I[Prompt Template];
    I --> J{Combine Query & Context};
    J --> K[Augmented Prompt];
    K --> L[LLM];
    L --> M{Generate Response};
    M --> N[Post-Processing];
    N --> O[Final Response];
    O --> P[User];

    subgraph Retrieval
        B
        C
        D
        E
        F
        G
    end

    subgraph Augmentation
        H
        I
        J
        K
    end

    subgraph Generation
        L
        M
        N
    end

    style A fill:#bbf,stroke:#333,stroke-width:2px
    style O fill:#bbf,stroke:#333,stroke-width:2px
    style F fill:#ccf,stroke:#333,stroke-width:1px
    style C fill:#ccf,stroke:#333,stroke-width:1px
    style L fill:#ccf,stroke:#333,stroke-width:1px
