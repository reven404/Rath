# RATH Technical Architecture

## System Architecture Diagram

```mermaid
graph TB
    subgraph "Frontend (React + TypeScript)"
        UI[User Interface]
        subgraph "Core Components"
            NAV[App Navigation]
            DS[Data Source]
            DC[Data Connection]
            MA[Mega Automation]
            SA[Semi Automation]
            MC[Manual Control]
            DP[Data Painter]
            DB[Dashboard]
            CA[Causal Analysis]
        end
        
        subgraph "State Management (MobX)"
            CS[Common Store]
            DSS[DataSource Store]
            MAS[MegaAuto Store]
            PS[Painter Store]
            DBS[Dashboard Store]
            CAS[Causal Store]
        end
        
        subgraph "Services & Utils"
            API[API Services]
            UTILS[Utilities]
            WORKERS[Web Workers]
        end
        
        subgraph "Visualization Engine"
            VR[Vega Renderer]
            VS[Vega Scenegraph]
            GW[Graphic Walker]
            CANVAS[Canvas/WebGL]
        end
    end
    
    subgraph "Backend Services (Microservices)"
        subgraph "Python Services"
            CAUSAL[Causal Service<br/>FastAPI + Algorithms]
            NARRATIVE[Narrative Service<br/>NLP + GPT]
            PREDICTION[Prediction Service<br/>ML Models]
            PATTERN[Text Pattern Service<br/>Text Analysis]
        end
        
        subgraph "Data Layer"
            CONNECTOR[Connector Service<br/>Database Abstraction]
            subgraph "Databases"
                POSTGRES[(PostgreSQL)]
                MYSQL[(MySQL)]
                CLICKHOUSE[(ClickHouse)]
                ATHENA[(Amazon Athena)]
                REDIS[(Redis Cache)]
            end
        end
    end
    
    subgraph "External Services"
        GPT[OpenAI GPT]
        CLOUD[Cloud Storage]
        CDN[CDN]
    end
    
    %% Connections
    UI --> NAV
    UI --> DS
    UI --> DC
    UI --> MA
    UI --> SA
    UI --> MC
    UI --> DP
    UI --> DB
    UI --> CA
    
    DS --> DSS
    MA --> MAS
    DP --> PS
    DB --> DBS
    CA --> CAS
    
    MA --> API
    SA --> API
    CA --> API
    DP --> API
    
    API --> CAUSAL
    API --> NARRATIVE
    API --> PREDICTION
    API --> PATTERN
    API --> CONNECTOR
    
    CONNECTOR --> POSTGRES
    CONNECTOR --> MYSQL
    CONNECTOR --> CLICKHOUSE
    CONNECTOR --> ATHENA
    
    VR --> CANVAS
    VS --> VR
    GW --> VS
    
    NARRATIVE --> GPT
    
    CAUSAL --> REDIS
    PREDICTION --> REDIS
    
    UI --> CDN
    
    classDef frontend fill:#e1f5fe
    classDef backend fill:#f3e5f5
    classDef database fill:#e8f5e8
    classDef external fill:#fff3e0
    
    class UI,NAV,DS,DC,MA,SA,MC,DP,DB,CA,CS,DSS,MAS,PS,DBS,CAS,API,UTILS,WORKERS,VR,VS,GW,CANVAS frontend
    class CAUSAL,NARRATIVE,PREDICTION,PATTERN,CONNECTOR backend
    class POSTGRES,MYSQL,CLICKHOUSE,ATHENA,REDIS database
    class GPT,CLOUD,CDN external
```

## Data Flow Architecture

```mermaid
flowchart TD
    subgraph "Data Input Layer"
        FILE[File Upload<br/>CSV/JSON/Excel]
        DB_CONN[Database Connection<br/>SQL Queries]
        API_CONN[API Connections<br/>REST/GraphQL]
    end
    
    subgraph "Data Processing Pipeline"
        LOAD[Data Loading]
        CLEAN[Data Cleaning]
        TRANSFORM[Data Transformation]
        PROFILE[Data Profiling]
        SAMPLE[Data Sampling]
    end
    
    subgraph "Analysis Engine"
        AUTO[AutoPilot Engine<br/>Pattern Discovery]
        SEMI[Copilot Engine<br/>Recommendations]
        MANUAL[Manual Analysis<br/>User Driven]
        CAUSAL_ENG[Causal Engine<br/>Discovery & Inference]
    end
    
    subgraph "Visualization Layer"
        CHART_GEN[Chart Generation]
        RENDER[Rendering Engine]
        INTERACT[Interaction Handler]
    end
    
    subgraph "Output Layer"
        DASHBOARD_OUT[Dashboard]
        EXPORT[Export/Share]
        INSIGHTS[Insights Report]
    end
    
    %% Data Flow
    FILE --> LOAD
    DB_CONN --> LOAD
    API_CONN --> LOAD
    
    LOAD --> CLEAN
    CLEAN --> TRANSFORM
    TRANSFORM --> PROFILE
    PROFILE --> SAMPLE
    
    SAMPLE --> AUTO
    SAMPLE --> SEMI
    SAMPLE --> MANUAL
    SAMPLE --> CAUSAL_ENG
    
    AUTO --> CHART_GEN
    SEMI --> CHART_GEN
    MANUAL --> CHART_GEN
    CAUSAL_ENG --> CHART_GEN
    
    CHART_GEN --> RENDER
    RENDER --> INTERACT
    
    INTERACT --> DASHBOARD_OUT
    INTERACT --> EXPORT
    INTERACT --> INSIGHTS
    
    classDef input fill:#e3f2fd
    classDef process fill:#f1f8e9
    classDef analysis fill:#fff3e0
    classDef viz fill:#fce4ec
    classDef output fill:#f3e5f5
    
    class FILE,DB_CONN,API_CONN input
    class LOAD,CLEAN,TRANSFORM,PROFILE,SAMPLE process
    class AUTO,SEMI,MANUAL,CAUSAL_ENG analysis
    class CHART_GEN,RENDER,INTERACT viz
    class DASHBOARD_OUT,EXPORT,INSIGHTS output
```

## Component Interaction Diagram

```mermaid
sequenceDiagram
    participant User
    participant UI
    participant Store
    participant Service
    participant Backend
    participant Database
    
    User->>UI: Upload Data
    UI->>Store: Update DataSource Store
    Store->>Service: Process Data
    Service->>Backend: Send Data for Analysis
    Backend->>Database: Store/Query Data
    Database-->>Backend: Return Results
    Backend-->>Service: Analysis Results
    Service->>Store: Update Analysis Store
    Store->>UI: Trigger Re-render
    UI-->>User: Display Insights
    
    User->>UI: Request AutoPilot
    UI->>Store: Trigger Mega Automation
    Store->>Service: Call Insights Service
    Service->>Backend: Request Pattern Discovery
    Backend->>Backend: Run ML Algorithms
    Backend-->>Service: Return Patterns
    Service->>Store: Update Insights
    Store->>UI: Update Visualization
    UI-->>User: Show Auto-Generated Charts
    
    User->>UI: Interact with Data Painter
    UI->>Store: Update Painter Store
    Store->>Service: Real-time Analysis
    Service-->>Store: Immediate Feedback
    Store->>UI: Update Canvas
    UI-->>User: Visual Feedback
```

## Technology Stack Detail

```mermaid
mindmap
  root((RATH Tech Stack))
    Frontend
      React 17
        TypeScript
        JSX/TSX
      State Management
        MobX
        Reactive Stores
      UI Framework
        Fluent UI
        Styled Components
      Visualization
        Vega/Vega-Lite
        Custom Renderer
        Canvas/WebGL
        D3.js concepts
    Backend
      Python Services
        FastAPI
        Async/Await
      Data Science
        NumPy/Pandas
        Scikit-learn
        NetworkX
        Causal-learn
        DoWhy
      ML/AI
        Pattern Recognition
        Causal Discovery
        Natural Language
    Infrastructure
      Build Tools
        Yarn Workspaces
        TypeScript Compiler
        React Scripts
      Deployment
        Docker
        Docker Compose
        Cloud Services
      Database
        PostgreSQL
        MySQL
        ClickHouse
        Redis
    Development
      Code Quality
        ESLint
        Prettier
        Jest Testing
      Monitoring
        Performance Tools
        Error Tracking
      Documentation
        Markdown
        API Docs
```

## Security Architecture

```mermaid
graph TB
    subgraph "Security Layers"
        subgraph "Frontend Security"
            CSP[Content Security Policy]
            XSS[XSS Protection]
            AUTH_F[Authentication]
        end
        
        subgraph "API Security"
            CORS[CORS Configuration]
            RATE[Rate Limiting]
            VALID[Input Validation]
            AUTH_API[API Authentication]
        end
        
        subgraph "Data Security"
            ENCRYPT[Data Encryption]
            BACKUP[Data Backup]
            PRIVACY[Privacy Controls]
        end
        
        subgraph "Infrastructure Security"
            HTTPS[HTTPS/TLS]
            FIREWALL[Firewall Rules]
            MONITOR[Security Monitoring]
        end
    end
    
    classDef security fill:#ffebee
    class CSP,XSS,AUTH_F,CORS,RATE,VALID,AUTH_API,ENCRYPT,BACKUP,PRIVACY,HTTPS,FIREWALL,MONITOR security
```

## Performance Optimization Strategy

```mermaid
graph LR
    subgraph "Frontend Optimization"
        CODE_SPLIT[Code Splitting]
        LAZY[Lazy Loading]
        MEMO[Memoization]
        VIRTUAL[Virtualization]
    end
    
    subgraph "Rendering Optimization"
        CANVAS_OPT[Canvas Optimization]
        WEBGL[WebGL Acceleration]
        WORKER[Web Workers]
        INCREMENTAL[Incremental Updates]
    end
    
    subgraph "Backend Optimization"
        CACHE[Caching Strategy]
        ASYNC[Async Processing]
        BATCH[Batch Operations]
        INDEX[Database Indexing]
    end
    
    subgraph "Data Optimization"
        SAMPLING[Smart Sampling]
        COMPRESSION[Data Compression]
        STREAMING[Data Streaming]
        PAGINATION[Pagination]
    end
    
    CODE_SPLIT --> PERFORMANCE[Better Performance]
    LAZY --> PERFORMANCE
    MEMO --> PERFORMANCE
    VIRTUAL --> PERFORMANCE
    
    CANVAS_OPT --> PERFORMANCE
    WEBGL --> PERFORMANCE
    WORKER --> PERFORMANCE
    INCREMENTAL --> PERFORMANCE
    
    CACHE --> PERFORMANCE
    ASYNC --> PERFORMANCE
    BATCH --> PERFORMANCE
    INDEX --> PERFORMANCE
    
    SAMPLING --> PERFORMANCE
    COMPRESSION --> PERFORMANCE
    STREAMING --> PERFORMANCE
    PAGINATION --> PERFORMANCE
    
    classDef optimization fill:#e8f5e8
    class CODE_SPLIT,LAZY,MEMO,VIRTUAL,CANVAS_OPT,WEBGL,WORKER,INCREMENTAL,CACHE,ASYNC,BATCH,INDEX,SAMPLING,COMPRESSION,STREAMING,PAGINATION optimization
```

This technical architecture documentation provides a comprehensive view of how RATH is structured and how its components interact with each other. The diagrams illustrate the system's complexity and the thoughtful design that enables its powerful data analysis capabilities.