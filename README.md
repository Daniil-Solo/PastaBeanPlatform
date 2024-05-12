## Pasta Bean Platform

### Архитектура

#### Диаграмма системного контекста
```mermaid
graph LR
    %% C4 style classes c4model.com %%
    classDef person fill:#08427b,stroke:black,color:white;
    classDef mainSystem fill:#1168bd,stroke:black,color:white;
    classDef existingSystem fill:#999999,stroke:black,color:white;
    classDef boundary fill:white,stroke:black,stroke-width:2px,stroke-dasharray: 5 5;
    classDef frame fill:white,stroke:black;

    %% nodes %%
    Person((User)):::person
    MainSystem[Pasta Bean <br>Platform]:::mainSystem
    ExistingSystem["GitHub"]:::existingSystem

    %% connections and boundaries %%
    subgraph Legend [System Context]
        Person-.->|Uses| MainSystem
        subgraph Boundary
            MainSystem
        end
        MainSystem-.->|Uses OAuth| ExistingSystem
        class Boundary boundary
    end
    class Legend frame
```

#### Диаграмма контейнеров
```mermaid
graph LR
    %% C4 style classes c4model.com %%
    classDef person fill:#08427b,stroke:black,color:white;
    classDef container fill:#1168bd,stroke:black,color:white;
    classDef database fill:#1168bd,stroke:black,color:white;
    classDef software fill:#1168bd,stroke:black,color:white;
    classDef existing fill:#999999,stroke:black,color:white;
    classDef boundary fill:white,stroke:black,stroke-width:2px,stroke-dasharray: 5 5;
    classDef frame fill:white,stroke:black;


    %% nodes %%
    ExistingSystem["GitHub"]:::existing
    Person((User)):::person
    WebApp("Web Application<br>[Container: nginx]"):::container
    APIGateway("API Gateway<br>[Container: Golang]"):::container
    AuthService("Auth Service<br>[Container: Python]"):::container
    UsersDB[("Users<br>[Container: PostgreSQL]")]:::database
    SessionCache[("Sessions<br>[Container: Redis]")]:::database
    EmailService("Email Service<br>[Container: Python]"):::container
    EmailEventTopic("Email Events<br>[Container: Apache Kafka Topic]"):::container
    PastaService("Pasta Service<br>[Container: Unknown]"):::container
    PastaStorage[("Pastas<br>[Container: Minio]")]:::database
    UGCService("UGC Service<br>[Container: Unknown]"):::container
    UGCDB[("UGC<br>[Container: MongoDB]")]:::database

    %% connections and boundaries %%
    
    subgraph Legend [Containers]
        Person-.->|Uses| WebApp
        subgraph Boundary["Boundary: System under development"]
            WebApp-.->|"Makes requests <br> [HTTP/HTTPS]"| APIGateway
            APIGateway-.->|"Makes requests <br> [HTTP/HTTPS]"|PastaService
            PastaService-.->|"Sends requests"|PastaStorage
            AuthService-.->|"Pushes events"|EmailEventTopic
            EmailService-.->|"Pulls events"|EmailEventTopic
            APIGateway-.->|"Makes requests <br> [HTTP/HTTPS]"|UGCService
            UGCService-.->|"Sends queries"|UGCDB
            APIGateway-.->|"Makes requests <br> [HTTP/HTTPS]"|AuthService
            AuthService-.->|"Sends queries"|UsersDB
            AuthService-.->|"Makes queries"|SessionCache
        end
        class Boundary boundary
        AuthService-.->|Uses OAuth| ExistingSystem
    end
    class Legend frame
```

### Модель данных
```mermaid
erDiagram
    Language {
        uuid id
        varchar name
    }
    Theme {
        uuid id
        varchar name
    }
    Pasta {
        uuid id
        varchar title
        text content
        uuid theme_id
        uuid language_id
        timestamp created_at
        timestamp updated_at
    }
    Pasta }o--|| Language : has
    Pasta }o--|| Theme : has

    User {
        uuid id
        varchar login
        varchar email
        varchar hashed_password
    }
    Profile {
        uuid user_id
        varchar fullname
        varchar description
        varchar avatar_link
    }
    User ||--|| Profile : has

    GuestPasta {
        uuid pasta_id
        varchar share_link
        varchar hashed_secret
    }
    GuestPasta ||--|| Pasta : extends

    UserPasta {
        uuid pasta_id
        uuid user_id
        bool is_public
    }
    UserPasta ||--|| Pasta : extends
    User ||--o{ UserPasta : creates

    Tag {
        uuid id
        uuid user_id
        varchar name
        timestamp created_at
    }
    TagInPasta {
        uuid pasta_id
        uuid tag_id
        timestamp created_at
    }
    Tag ||--o{ TagInPasta : "includes in"
    UserPasta ||--o{ TagInPasta : has
    User ||--o{ Tag : creates

    Like {
        uuid pasta_id
        uuid user_id
    }
    User ||--o{ Like : puts
    UserPasta ||--o{ Like : has

    Comment {
        uuid id
        uuid user_id
        uuid pasta_id
        uuid parent_comment_id
        varchar text
        
    }
    User ||--o{ Comment : creates
    UserPasta ||--o{ Comment : has
```
