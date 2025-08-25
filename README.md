# 🏗️ Kompletna Architektura Projektu - Document Processing API

## 📋 Spis Treści
1. [Struktura Katalogów](#struktura-katalogów)
2. [Diagramy Architektury](#diagramy-architektury)
3. [Szczegółowa Architektura Komponentów](#szczegółowa-architektura-komponentów)
4. [Przepływy Danych](#przepływy-danych)
5. [Baza Danych](#baza-danych)
6. [Konfiguracja i Deployment](#konfiguracja-i-deployment)
7. [Bezpieczeństwo](#bezpieczeństwo)
8. [Monitoring i Obserwabilność](#monitoring-i-obserwabilność)
9. [Skalowanie](#skalowanie)
10. [Troubleshooting](#troubleshooting)

---

## 🌳 Struktura Katalogów

```
document-processing-api/
│
├── 📁 backend/
│   └── 📁 app/
│       ├── 📄 __init__.py
│       ├── 📄 main.py                    # 🚀 FastAPI application entry point
│       ├── 📄 celery_app.py              # 📋 Celery configuration & task routing
│       ├── 📄 database.py                # 🗄️ Database connection & session management
│       ├── 📄 schemas.py                 # 📝 Pydantic models for API validation
│       ├── 📄 dependencies.py            # 🔧 Dependency injection utilities
│       ├── 📄 exceptions.py              # 🚨 Custom exception handlers
│       ├── 📄 config.py                  # ⚙️ Configuration management
│       │
│       ├── 📁 api/
│       │   ├── 📄 __init__.py
│       │   ├── 📄 documents.py           # 📄 Document-related endpoints
│       │   ├── 📄 tasks.py              # 📋 Task management endpoints
│       │   ├── 📄 search.py             # 🔍 Search & query endpoints
│       │   ├── 📄 health.py             # 💚 Health check endpoints
│       │   └── 📄 analytics.py          # 📊 Analytics endpoints
│       │
│       ├── 📁 models/
│       │   ├── 📄 __init__.py
│       │   ├── 📄 base.py               # 🏗️ Base model with common fields
│       │   ├── 📄 document.py           # 📄 Document SQLAlchemy model
│       │   ├── 📄 task.py               # 📋 Task SQLAlchemy model
│       │   ├── 📄 user.py               # 👤 User model (for auth)
│       │   ├── 📄 audit.py              # 📝 Audit trail model
│       │   └── 📄 analytics.py          # 📊 Analytics & metrics models
│       │
│       ├── 📁 services/
│       │   ├── 📄 __init__.py
│       │   ├── 📄 document_service.py    # 📄 Document processing business logic
│       │   ├── 📄 vector_service.py      # 🧠 Weaviate integration & vector operations
│       │   ├── 📄 transcription_service.py # 🎤 Audio transcription with Whisper
│       │   ├── 📄 embedding_service.py   # 🔢 Embedding generation & management
│       │   ├── 📄 search_service.py      # 🔍 Search & similarity operations
│       │   ├── 📄 notification_service.py # 📧 Notification management
│       │   ├── 📄 cache_service.py       # 💾 Caching strategies
│       │   ├── 📄 audit_service.py       # 📝 Audit logging service
│       │   └── 📄 analytics_service.py   # 📊 Analytics & reporting
│       │
│       ├── 📁 workers/
│       │   ├── 📄 __init__.py
│       │   ├── 📄 document_processor.py  # 📄 Document processing worker
│       │   ├── 📄 transcription.py       # 🎤 Audio transcription worker
│       │   ├── 📄 embedding_generator.py # 🔢 Embedding generation worker
│       │   ├── 📄 batch_processor.py     # 📦 Batch processing worker
│       │   ├── 📄 cleanup_worker.py      # 🧹 Cleanup & maintenance tasks
│       │   └── 📄 notification_worker.py # 📧 Notification dispatch worker
│       │
│       ├── 📁 core/
│       │   ├── 📄 __init__.py
│       │   ├── 📄 security.py           # 🔐 Authentication & authorization
│       │   ├── 📄 middleware.py         # 🔄 Custom middleware
│       │   ├── 📄 logging_config.py     # 📋 Structured logging setup
│       │   ├── 📄 metrics.py            # 📊 Application metrics
│       │   └── 📄 rate_limiting.py      # 🚦 Rate limiting implementation
│       │
│       ├── 📁 utils/
│       │   ├── 📄 __init__.py
│       │   ├── 📄 file_handler.py       # 📁 File operations utilities
│       │   ├── 📄 text_processor.py     # ✂️ Text chunking & preprocessing
│       │   ├── 📄 validators.py         # ✅ Custom validation functions
│       │   ├── 📄 formatters.py         # 🎨 Data formatting utilities
│       │   ├── 📄 encryption.py         # 🔒 Encryption utilities
│       │   └── 📄 helpers.py            # 🔧 General helper functions
│       │
│       └── 📁 tests/
│           ├── 📄 __init__.py
│           ├── 📄 conftest.py           # 🧪 Pytest configuration
│           ├── 📁 unit/                 # 🔬 Unit tests
│           ├── 📁 integration/          # 🔄 Integration tests
│           ├── 📁 e2e/                 # 🌐 End-to-end tests
│           └── 📁 fixtures/             # 📋 Test data fixtures
│
├── 📁 uploads/                          # 📂 File upload directory
│   ├── 📁 documents/                    # 📄 Uploaded documents
│   ├── 📁 audio/                        # 🎤 Audio files
│   ├── 📁 images/                       # 🖼️ Image files
│   └── 📁 temp/                         # 🗂️ Temporary processing files
│
├── 📁 scripts/                          # 🔧 Utility scripts
│   ├── 📄 setup_db.py                  # 🗄️ Database initialization
│   ├── 📄 migrate_data.py              # 🔄 Data migration scripts
│   ├── 📄 cleanup_old_files.py         # 🧹 File cleanup utilities
│   ├── 📄 backup_restore.py            # 💾 Backup & restore tools
│   └── 📄 health_check.py              # 💚 System health verification
│
├── 📁 docs/                            # 📚 Documentation
│   ├── 📄 API_REFERENCE.md             # 📖 API documentation
│   ├── 📄 DEPLOYMENT.md                # 🚀 Deployment guide
│   ├── 📄 ARCHITECTURE.md              # 🏗️ Architecture overview
│   └── 📁 diagrams/                    # 📊 Architecture diagrams
│
├── 📁 monitoring/                      # 📊 Monitoring configuration
│   ├── 📄 prometheus.yml               # 📈 Prometheus config
│   ├── 📄 grafana-dashboard.json       # 📊 Grafana dashboard
│   └── 📄 alerts.yml                   # 🚨 Alert rules
│
├── 📁 nginx/                           # 🌐 Reverse proxy configuration
│   ├── 📄 nginx.conf                   # ⚙️ Nginx main config
│   ├── 📄 sites-enabled/               # 🌍 Virtual host configs
│   └── 📄 ssl/                         # 🔒 SSL certificates
│
├── 📄 docker-compose.yml               # 🐳 Development environment
├── 📄 docker-compose.prod.yml          # 🚀 Production environment
├── 📄 docker-compose.monitoring.yml    # 📊 Monitoring stack
├── 📄 Dockerfile                       # 🐳 Application container
├── 📄 Dockerfile.worker                # 👷 Worker container
├── 📄 requirements.txt                 # 🐍 Python dependencies
├── 📄 requirements-dev.txt             # 🛠️ Development dependencies
├── 📄 .env.example                     # 📋 Environment template
├── 📄 .dockerignore                    # 🚫 Docker ignore rules
├── 📄 .gitignore                       # 🚫 Git ignore rules
├── 📄 Makefile                         # 🔨 Build automation
├── 📄 pyproject.toml                   # 📦 Python project config
└── 📄 README.md                        # 📖 Project documentation
```

---

## 🏗️ Diagramy Architektury

### 1. 🌐 Architektura Systemu - Widok Ogólny

```mermaid
graph TB
    subgraph "🌐 External Layer"
        Users[👥 Users]
        APIs[🔗 External APIs]
        CDN[🚀 CDN]
    end
    
    subgraph "🛡️ Security Layer"
        WAF[🛡️ Web Application Firewall]
        RateLimit[🚦 Rate Limiter]
        Auth[🔐 Authentication Service]
    end
    
    subgraph "🌍 Load Balancer Layer"
        LB[⚖️ Load Balancer<br/>Nginx + SSL]
        HealthCheck[💚 Health Check]
    end
    
    subgraph "🚀 Application Layer"
        API1[📱 FastAPI App 1<br/>Port: 8001]
        API2[📱 FastAPI App 2<br/>Port: 8002]
        API3[📱 FastAPI App N<br/>Port: 800N]
        
        Worker1[👷 Document Worker<br/>Queue: default]
        Worker2[🎤 Transcription Worker<br/>Queue: high_priority]
        Worker3[🔢 Embedding Worker<br/>Queue: low_priority]
        WorkerN[🧹 Cleanup Worker<br/>Queue: maintenance]
    end
    
    subgraph "📨 Message Queue Layer"
        Redis[(🔴 Redis Cluster<br/>Port: 6379)]
        Queue1[📋 Default Queue<br/>Document Processing]
        Queue2[⚡ High Priority Queue<br/>Real-time Tasks]
        Queue3[🐌 Low Priority Queue<br/>Background Tasks]
        Queue4[🛠️ Maintenance Queue<br/>Cleanup & Admin]
    end
    
    subgraph "💾 Storage Layer"
        Primary[(🗄️ PostgreSQL Primary<br/>Port: 5432)]
        Replica1[(📖 PostgreSQL Replica 1<br/>Read Only)]
        Replica2[(📖 PostgreSQL Replica 2<br/>Read Only)]
        
        VectorPrimary[(🧠 Weaviate Primary<br/>Port: 8080)]
        VectorReplica[(🧠 Weaviate Replica<br/>Port: 8081)]
        
        Files[📁 File Storage<br/>S3 Compatible]
        Cache[💾 Redis Cache<br/>Port: 6380]
    end
    
    subgraph "📊 Monitoring Layer"
        Prometheus[📈 Prometheus]
        Grafana[📊 Grafana Dashboard]
        AlertManager[🚨 Alert Manager]
        Jaeger[🔍 Jaeger Tracing]
    end
    
    Users --> CDN
    CDN --> WAF
    WAF --> RateLimit
    RateLimit --> Auth
    Auth --> LB
    
    LB --> API1
    LB --> API2
    LB --> API3
    
    API1 --> Redis
    API2 --> Redis
    API3 --> Redis
    
    API1 --> Primary
    API2 --> Replica1
    API3 --> Replica2
    
    API1 --> Cache
    API2 --> Cache
    API3 --> Cache
    
    Redis --> Queue1
    Redis --> Queue2
    Redis --> Queue3
    Redis --> Queue4
    
    Queue1 --> Worker1
    Queue2 --> Worker2
    Queue3 --> Worker3
    Queue4 --> WorkerN
    
    Worker1 --> Primary
    Worker2 --> Primary
    Worker3 --> VectorPrimary
    WorkerN --> Primary
    
    Worker1 --> Files
    Worker2 --> Files
    Worker3 --> Files
    
    Worker1 --> VectorPrimary
    Worker3 --> VectorReplica
    
    API1 --> Prometheus
    Worker1 --> Prometheus
    Prometheus --> Grafana
    Prometheus --> AlertManager
    
    API1 --> Jaeger
    Worker1 --> Jaeger
```

### 2. 🔄 Szczegółowy Flow Przetwarzania Dokumentów

```mermaid
sequenceDiagram
    participant Client as 👤 Client
    participant LB as ⚖️ Load Balancer
    participant API as 📱 FastAPI
    participant Auth as 🔐 Auth Service
    participant DB as 🗄️ PostgreSQL
    participant Redis as 🔴 Redis
    participant Worker as 👷 Worker
    participant Vector as 🧠 Weaviate
    participant Files as 📁 File Storage
    participant Cache as 💾 Cache
    participant Notify as 📧 Notification
    
    Note over Client: Document Upload Process
    
    Client->>+LB: POST /documents/upload
    LB->>+API: Forward Request
    API->>+Auth: Validate Token
    Auth->>-API: Token Valid ✅
    
    API->>+Files: Save Uploaded File
    Files->>-API: File Saved ✅
    
    API->>+DB: Create Document Record
    DB->>-API: Document Created (ID: 123)
    
    API->>+Redis: Queue Processing Task
    Note over Redis: Task queued based on file type<br/>PDF/DOCX → default<br/>Audio → high_priority<br/>Images → low_priority
    Redis->>-API: Task Queued (Task ID: abc-123)
    
    API->>+Cache: Cache Document Metadata
    Cache->>-API: Cached ✅
    
    API->>-LB: Return Response
    LB->>-Client: 201 Created<br/>{document_id: 123, task_id: "abc-123"}
    
    Note over Worker: Async Processing Begins
    
    Redis->>+Worker: Deliver Task
    Worker->>+DB: Update Status: PROCESSING
    DB->>-Worker: Updated ✅
    
    Worker->>+Files: Read File Content
    Files->>-Worker: File Content
    
    alt Document Processing
        Worker->>Worker: Extract Text
        Worker->>Worker: Chunk Text (1000 chars, 200 overlap)
        Worker->>+Vector: Store Document Chunks
        Vector->>-Worker: Vectors Stored (weaviate_id: vec-123)
    else Audio Processing
        Worker->>Worker: Whisper Transcription
        Worker->>+DB: Store Transcription
        DB->>-Worker: Transcription Saved ✅
    else Image Processing
        Worker->>Worker: Generate Image Embeddings
        Worker->>+Vector: Store Image Vectors
        Vector->>-Worker: Vectors Stored ✅
    end
    
    Worker->>+DB: Update Document Status: PROCESSED
    DB->>-Worker: Status Updated ✅
    
    Worker->>+DB: Create Task Success Record
    DB->>-Worker: Task Record Created ✅
    
    Worker->>+Cache: Invalidate Document Cache
    Cache->>-Worker: Cache Invalidated ✅
    
    Worker->>+Notify: Send Completion Notification
    Notify->>-Worker: Notification Queued ✅
    
    Note over Client: Status Polling
    
    Client->>+LB: GET /documents/123
    LB->>+API: Forward Request
    API->>+Cache: Check Cache
    Cache->>-API: Cache Miss ❌
    
    API->>+DB: Query Document Status
    DB->>-API: Document Data
    
    API->>+Cache: Update Cache (TTL: 5min)
    Cache->>-API: Cached ✅
    
    API->>-LB: Return Status
    LB->>-Client: 200 OK<br/>{status: "processed", weaviate_id: "vec-123"}
```

### 3. 🧩 Szczegółowa Architektura Komponentów

```mermaid
graph TB
    subgraph "🌐 API Gateway Layer"
        Router[🛤️ FastAPI Router]
        Middleware[🔄 Middleware Stack]
        CORS[🌍 CORS Handler]
        RateLimit[🚦 Rate Limiter]
        Auth[🔐 JWT Authenticator]
        Validation[✅ Request Validator]
        Logging[📋 Request Logger]
    end
    
    subgraph "📱 API Endpoints"
        DocAPI[📄 Documents API<br/>/api/v1/documents]
        TaskAPI[📋 Tasks API<br/>/api/v1/tasks]
        SearchAPI[🔍 Search API<br/>/api/v1/search]
        HealthAPI[💚 Health API<br/>/health]
        MetricsAPI[📊 Metrics API<br/>/metrics]
        AdminAPI[⚙️ Admin API<br/>/admin]
    end
    
    subgraph "🧠 Business Logic Services"
        DocService[📄 Document Service<br/>• File validation<br/>• Metadata extraction<br/>• Type detection]
        
        VectorService[🧠 Vector Service<br/>• Embedding generation<br/>• Similarity search<br/>• Vector operations]
        
        TranscriptionService[🎤 Transcription Service<br/>• Audio preprocessing<br/>• Whisper integration<br/>• Language detection]
        
        SearchService[🔍 Search Service<br/>• Full-text search<br/>• Semantic search<br/>• Result ranking]
        
        CacheService[💾 Cache Service<br/>• Redis integration<br/>• TTL management<br/>• Cache invalidation]
        
        NotifyService[📧 Notification Service<br/>• Email notifications<br/>• Webhook callbacks<br/>• Status updates]
    end
    
    subgraph "📊 Data Access Layer"
        DocumentRepo[📄 Document Repository<br/>• CRUD operations<br/>• Query builders<br/>• Relationships]
        
        TaskRepo[📋 Task Repository<br/>• Status tracking<br/>• Progress updates<br/>• Error handling]
        
        UserRepo[👤 User Repository<br/>• Authentication<br/>• Authorization<br/>• Profile management]
        
        AnalyticsRepo[📊 Analytics Repository<br/>• Usage metrics<br/>• Performance data<br/>• Reporting queries]
    end
    
    subgraph "👷 Worker Processes"
        DocWorker[📄 Document Processor<br/>• PDF extraction<br/>• Text chunking<br/>• Metadata parsing]
        
        AudioWorker[🎤 Audio Processor<br/>• Format conversion<br/>• Noise reduction<br/>• Transcription]
        
        EmbedWorker[🔢 Embedding Generator<br/>• Vector creation<br/>• Dimensionality reduction<br/>• Index updating]
        
        BatchWorker[📦 Batch Processor<br/>• Bulk operations<br/>• Data migrations<br/>• Report generation]
        
        CleanupWorker[🧹 Cleanup Worker<br/>• Temp file removal<br/>• Log rotation<br/>• Cache cleanup]
    end
    
    Router --> Middleware
    Middleware --> CORS
    CORS --> RateLimit
    RateLimit --> Auth
    Auth --> Validation
    Validation --> Logging
    
    Logging --> DocAPI
    Logging --> TaskAPI
    Logging --> SearchAPI
    Logging --> HealthAPI
    Logging --> MetricsAPI
    Logging --> AdminAPI
    
    DocAPI --> DocService
    TaskAPI --> DocService
    SearchAPI --> SearchService
    
    DocService --> VectorService
    DocService --> TranscriptionService
    SearchService --> VectorService
    
    DocService --> CacheService
    VectorService --> CacheService
    
    DocService --> NotifyService
    
    DocService --> DocumentRepo
    TaskAPI --> TaskRepo
    SearchAPI --> AnalyticsRepo
    
    DocumentRepo --> DocWorker
    TaskRepo --> AudioWorker
    DocumentRepo --> EmbedWorker
    AnalyticsRepo --> BatchWorker
    TaskRepo --> CleanupWorker
```

### 4. 📊 Przepływ Danych w Systemie

```mermaid
flowchart TD
    A[📤 File Upload] --> B{🔍 File Type Detection}
    
    B -->|📄 PDF/DOCX/TXT| C[📋 Document Processing Queue]
    B -->|🎤 Audio Files| D[⚡ High Priority Queue]
    B -->|🖼️ Images| E[🐌 Low Priority Queue]
    B -->|📦 Archive Files| F[📦 Batch Processing Queue]
    
    C --> G[📄 Document Processor]
    D --> H[🎤 Audio Processor]
    E --> I[🖼️ Image Processor]
    F --> J[📦 Batch Processor]
    
    G --> G1[📝 Text Extraction]
    G1 --> G2[✂️ Text Chunking]
    G2 --> G3[🔢 Embedding Generation]
    G3 --> G4[🧠 Vector Storage]
    
    H --> H1[🔊 Audio Preprocessing]
    H1 --> H2[🎤 Whisper Transcription]
    H2 --> H3[🌍 Language Detection]
    H3 --> H4[📝 Text Post-processing]
    
    I --> I1[🖼️ Image Preprocessing]
    I1 --> I2[👁️ Feature Extraction]
    I2 --> I3[🔢 Image Embeddings]
    I3 --> I4[🧠 Vector Storage]
    
    J --> J1[📦 Archive Extraction]
    J1 --> J2[📁 File Enumeration]
    J2 --> J3[🔄 Recursive Processing]
    J3 --> A
    
    G4 --> K[💾 Cache Update]
    H4 --> K
    I4 --> K
    
    K --> L[📧 Notification Dispatch]
    L --> M[✅ Status Update: PROCESSED]
    
    M --> N{🔍 Quality Check}
    N -->|✅ Pass| O[🎉 Processing Complete]
    N -->|❌ Fail| P[🔄 Retry Processing]
    P --> C
    
    O --> Q[📊 Analytics Update]
    Q --> R[📈 Metrics Collection]
    
    subgraph "🚨 Error Handling"
        Error1[❌ Extraction Failed]
        Error2[❌ Transcription Failed]
        Error3[❌ Embedding Failed]
        
        Error1 --> Retry1[🔄 Retry Logic]
        Error2 --> Retry2[🔄 Retry Logic]
        Error3 --> Retry3[🔄 Retry Logic]
        
        Retry1 --> DLQ[💀 Dead Letter Queue]
        Retry2 --> DLQ
        Retry3 --> DLQ
        
        DLQ --> Manual[👨‍💻 Manual Review]
    end
    
    G1 --> Error1
    H2 --> Error2
    G3 --> Error3
```

---

## 📚 Szczegółowa Architektura Komponentów

### 🔧 Core Services

#### 📄 Document Service
```python
class DocumentService:
    """
    Główny serwis do zarządzania dokumentami
    
    Funkcjonalności:
    • Walidacja i detekcja typu plików
    • Ekstrakcja metadanych (autor, data utworzenia, rozmiar)
    • Konwersja formatów dokumentów
    • Generowanie miniatur i podglądów
    • Zarządzanie cyklem życia dokumentu
    """
    
    SUPPORTED_FORMATS = {
        'documents': ['.pdf', '.docx', '.doc', '.txt', '.rtf', '.odt'],
        'audio': ['.mp3', '.wav', '.ogg', '.m4a', '.flac'],
        'images': ['.jpg', '.jpeg', '.png', '.bmp', '.tiff', '.svg'],
        'archives': ['.zip', '.tar', '.gz', '.rar', '.7z']
    }
    
    MAX_FILE_SIZE = {
        'documents': 100 * 1024 * 1024,  # 100MB
        'audio': 500 * 1024 * 1024,      # 500MB
        'images': 50 * 1024 * 1024,      # 50MB
        'archives': 1024 * 1024 * 1024   # 1GB
    }
```

#### 🧠 Vector Service
```python
class VectorService:
    """
    Serwis do zarządzania wektorami i operacjami wyszukiwania
    
    Funkcjonalności:
    • Generowanie embeddings dla tekstu i obrazów
    • Przechowywanie wektorów w Weaviate
    • Wyszukiwanie semantyczne
    • Klastrowanie podobnych dokumentów
    • Rekomendacje na podstawie podobieństwa
    """
    
    EMBEDDING_MODELS = {
        'text': 'all-MiniLM-L6-v2',      # 384 dimensions
        'multilingual': 'paraphrase-multilingual-MiniLM-L12-v2',
        'image': 'clip-ViT-B-32',        # 512 dimensions
        'code': 'microsoft/codebert-base' # 768 dimensions
    }
    
    SIMILARITY_THRESHOLD = 0.7
    MAX_RESULTS = 100
```

#### 🎤 Transcription Service
```python
class TranscriptionService:
    """
    Serwis transkrypcji audio przy użyciu OpenAI Whisper
    
    Funkcjonalności:
    • Automatyczna detekcja języka
    • Transkrypcja z znacznikami czasu
    • Identyfikacja głośników (speaker diarization)
    • Filtrowanie szumów i normalizacja audio
    • Generowanie podsumowań transkrypcji
    """
    
    WHISPER_MODELS = {
        'tiny': {'size': '39MB', 'speed': 'fastest', 'accuracy': 'lowest'},
        'base': {'size': '74MB', 'speed': 'fast', 'accuracy': 'medium'},
        'small': {'size': '244MB', 'speed': 'medium', 'accuracy': 'good'},
        'medium': {'size': '769MB', 'speed': 'slow', 'accuracy': 'better'},
        'large': {'size': '1550MB', 'speed': 'slowest', 'accuracy': 'best'}
    }
    
    SUPPORTED_LANGUAGES = ['en', 'pl', 'de', 'fr', 'es', 'it', 'ru', 'zh']
```

### 👷 Workers Architecture

#### 📄 Document Processing Worker
```mermaid
flowchart TD
    A[📥 Task Received] --> B[📋 Validate Input]
    B --> C{📄 File Type?}
    
    C -->|PDF| D[📕 PDF Processor]
    C -->|DOCX| E[📘 Word Processor]
    C -->|TXT| F[📄 Text Processor]
    C -->|HTML| G[🌐 HTML Processor]
    
    D --> D1[🔍 Extract Text & Images]
    D1 --> D2[📊 Extract Metadata]
    D2 --> D3[🎨 Generate Thumbnails]
    
    E --> E1[📝 Extract Text & Tables]
    E1 --> E2[📊 Extract Styles & Formatting]
    E2 --> E3[🖼️ Extract Embedded Media]
    
    F --> F1[✂️ Text Chunking]
    G --> G1[🏷️ HTML Parsing & Cleanup]
    
    D3 --> H[✂️ Text Chunking]
    E3 --> H
    F1 --> H
    G1 --> H
    
    H --> I[🔢 Generate Embeddings]
    I --> J[🧠 Store in Weaviate]
    J --> K[💾 Update Database]
    K --> L[📧 Send Notification]
    L --> M[✅ Task Complete]
    
    subgraph "🚨 Error Handling"
        N[❌ Processing Error]
        O[🔄 Retry Logic<br/>Max: 3 attempts]
        P[💀 Dead Letter Queue]
        
        N --> O
        O -->|Failed| P
        O -->|Success| H
    end
    
    D --> N
    E --> N
    F --> N
    G --> N
```

#### 🎤 Audio Processing Worker
```mermaid
flowchart TD
    A[📥 Audio Task] --> B[🔊 Audio Validation]
    B --> C[📊 Analyze Audio Properties]
    
    C --> D{🎵 Audio Quality?}
    D -->|Good| E[🎤 Direct Transcription]
    D -->|Poor| F[🔧 Audio Enhancement]
    
    F --> F1[🔇 Noise Reduction]
    F1 --> F2[📢 Volume Normalization]
    F2 --> F3[🎛️ Format Conversion]
    F3 --> E
    
    E --> G[🌍 Language Detection]
    G --> H{🗣️ Multiple Speakers?}
    
    H -->|Yes| I[👥 Speaker Diarization]
    H -->|No| J[🎤 Single Speaker Transcription]
    
    I --> I1[🔍 Voice Activity Detection]
    I1 --> I2[👤 Speaker Identification]
    I2 --> I3[⏱️ Timestamp Alignment]
    I3 --> K[📝 Multi-Speaker Transcript]
    
    J --> J1[🎤 Whisper Transcription]
    J1 --> J2[⏱️ Timestamp Generation]
    J2 --> L[📝 Single Speaker Transcript]
    
    K --> M[🔍 Post-Processing]
    L --> M
    
    M --> M1[✏️ Grammar Correction]
    M1 --> M2[🔤 Spell Check]
    M2 --> M3[📖 Punctuation Enhancement]
    M3 --> N[💾 Store Transcription]
    
    N --> O[🔢 Generate Text Embeddings]
    O --> P[🧠 Store in Vector DB]
    P --> Q[📊 Update Analytics]
    Q --> R[✅ Task Complete]
    
    subgraph "🚨 Audio Processing Errors"
        S[❌ Transcription Failed]
        T[🔄 Retry with Different Model]
        U[📞 Human Review Queue]
        
        S --> T
        T -->|Max Retries| U
        T -->|Success| M
    end
    
    E --> S
    J1 --> S

#### 🔢 Embedding Generation Worker
```mermaid
flowchart TD
    A[📥 Embedding Task] --> B[📊 Analyze Content Type]
    B --> C{📝 Content Type?}
    
    C -->|Text| D[📄 Text Embedding Pipeline]
    C -->|Image| E[🖼️ Image Embedding Pipeline]
    C -->|Code| F[💻 Code Embedding Pipeline]
    C -->|Audio| G[🎤 Audio Feature Extraction]
    
    D --> D1[🔤 Text Preprocessing]
    D1 --> D2[✂️ Text Chunking]
    D2 --> D3[🧠 Generate Text Vectors]
    D3 --> H[🔍 Quality Validation]
    
    E --> E1[🖼️ Image Preprocessing]
    E1 --> E2[🎨 Feature Extraction]
    E2 --> E3[🧠 Generate Image Vectors]
    E3 --> H
    
    F --> F1[💻 Code Analysis]
    F1 --> F2[🏷️ Syntax Highlighting]
    F2 --> F3[🧠 Generate Code Vectors]
    F3 --> H
    
    G --> G1[🎵 Audio Feature Analysis]
    G1 --> G2[📊 Spectral Analysis]
    G2 --> G3[🧠 Generate Audio Vectors]
    G3 --> H
    
    H --> I{✅ Quality OK?}
    I -->|Yes| J[🧠 Store in Vector DB]
    I -->|No| K[🔄 Regenerate with Different Model]
    
    K --> L{🔢 Retry Count < 3?}
    L -->|Yes| D
    L -->|No| M[❌ Mark as Failed]
    
    J --> N[🏷️ Create Vector Index]
    N --> O[🔗 Link to Original Document]
    O --> P[📊 Update Metadata]
    P --> Q[✅ Task Complete]
    
    M --> R[📧 Admin Notification]
    R --> S[📝 Error Log]
```

---

## 💾 Szczegółowa Architektura Bazy Danych

### 🗄️ PostgreSQL Schema Design

```mermaid
erDiagram
    USERS {
        uuid id PK
        string email UK
        string username UK
        string password_hash
        string first_name
        string last_name
        enum role
        jsonb preferences
        boolean is_active
        datetime last_login
        datetime created_at
        datetime updated_at
        string created_by FK
        string updated_by FK
    }
    
    DOCUMENTS {
        bigint id PK
        uuid user_id FK
        string filename
        string original_filename
        string file_path
        string content_type
        bigint file_size
        string file_hash
        enum status
        string task_id
        string weaviate_id
        text transcription
        jsonb metadata
        jsonb processing_config
        integer chunk_count
        float processing_time
        datetime uploaded_at
        datetime processed_at
        datetime created_at
        datetime updated_at
        string created_by FK
        string updated_by FK
    }
    
    DOCUMENT_CHUNKS {
        bigint id PK
        bigint document_id FK
        integer chunk_index
        text content
        integer start_position
        integer end_position
        float confidence_score
        jsonb metadata
        string weaviate_chunk_id
        datetime created_at
        datetime updated_at
    }
    
    TASKS {
        bigint id PK
        string task_id UK
        uuid user_id FK
        bigint document_id FK
        string task_type
        enum status
        enum priority
        jsonb input_data
        jsonb result_data
        jsonb progress_data
        text error_message
        string error_code
        integer retry_count
        integer max_retries
        datetime started_at
        datetime completed_at
        datetime next_retry_at
        float processing_time
        string worker_id
        datetime created_at
        datetime updated_at
    }
    
    SEARCH_QUERIES {
        bigint id PK
        uuid user_id FK
        text query
        jsonb filters
        jsonb results_metadata
        integer results_count
        float response_time
        enum search_type
        datetime created_at
    }
    
    AUDIT_LOGS {
        bigint id PK
        uuid user_id FK
        string entity_type
        string entity_id
        string action
        jsonb old_values
        jsonb new_values
        string ip_address
        string user_agent
        datetime created_at
    }
    
    ANALYTICS_EVENTS {
        bigint id PK
        uuid user_id FK
        string event_type
        string entity_type
        string entity_id
        jsonb event_data
        string session_id
        string ip_address
        datetime created_at
    }
    
    NOTIFICATIONS {
        bigint id PK
        uuid user_id FK
        bigint document_id FK
        bigint task_id FK
        string type
        string title
        text message
        jsonb data
        boolean is_read
        datetime read_at
        datetime created_at
        datetime updated_at
    }
    
    API_KEYS {
        bigint id PK
        uuid user_id FK
        string key_hash
        string name
        jsonb permissions
        integer rate_limit
        datetime last_used_at
        datetime expires_at
        boolean is_active
        datetime created_at
        datetime updated_at
    }
    
    SYSTEM_METRICS {
        bigint id PK
        string metric_name
        string metric_type
        float value
        jsonb labels
        datetime timestamp
        datetime created_at
    }
    
    USERS ||--o{ DOCUMENTS : "owns"
    USERS ||--o{ TASKS : "initiates"
    USERS ||--o{ SEARCH_QUERIES : "performs"
    USERS ||--o{ AUDIT_LOGS : "generates"
    USERS ||--o{ ANALYTICS_EVENTS : "triggers"
    USERS ||--o{ NOTIFICATIONS : "receives"
    USERS ||--o{ API_KEYS : "has"
    
    DOCUMENTS ||--o{ DOCUMENT_CHUNKS : "contains"
    DOCUMENTS ||--o{ TASKS : "processes"
    DOCUMENTS ||--o{ NOTIFICATIONS : "relates_to"
    
    TASKS ||--o{ NOTIFICATIONS : "generates"
```

### 📊 Database Indexes Strategy

```sql
-- Performance Critical Indexes
CREATE INDEX CONCURRENTLY idx_documents_user_status 
ON documents(user_id, status) 
WHERE status IN ('pending', 'processing');

CREATE INDEX CONCURRENTLY idx_documents_content_type 
ON documents(content_type, created_at DESC);

CREATE INDEX CONCURRENTLY idx_tasks_status_priority 
ON tasks(status, priority, created_at) 
WHERE status IN ('pending', 'processing');

CREATE INDEX CONCURRENTLY idx_document_chunks_document_embedding 
ON document_chunks(document_id, weaviate_chunk_id) 
WHERE weaviate_chunk_id IS NOT NULL;

-- Full Text Search Indexes
CREATE INDEX CONCURRENTLY idx_documents_search 
ON documents USING gin(to_tsvector('english', filename || ' ' || COALESCE(metadata->>'title', '')));

CREATE INDEX CONCURRENTLY idx_document_chunks_search 
ON document_chunks USING gin(to_tsvector('english', content));

-- Analytics Indexes
CREATE INDEX CONCURRENTLY idx_analytics_events_user_time 
ON analytics_events(user_id, created_at DESC);

CREATE INDEX CONCURRENTLY idx_search_queries_performance 
ON search_queries(created_at DESC, response_time);

-- Audit Indexes
CREATE INDEX CONCURRENTLY idx_audit_logs_entity 
ON audit_logs(entity_type, entity_id, created_at DESC);

-- Partitioning Strategy for Large Tables
CREATE TABLE analytics_events_y2024m01 PARTITION OF analytics_events 
FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');

CREATE TABLE audit_logs_y2024m01 PARTITION OF audit_logs 
FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');
```

---

## 🧠 Weaviate Vector Database Architecture

### 📚 Schema Definition

```python
WEAVIATE_SCHEMA = {
    "classes": [
        {
            "class": "Document",
            "description": "Processed document with metadata and embeddings",
            "vectorizer": "text2vec-transformers",
            "moduleConfig": {
                "text2vec-transformers": {
                    "poolingStrategy": "masked_mean",
                    "model": "sentence-transformers/all-MiniLM-L6-v2"
                },
                "generative-openai": {
                    "model": "gpt-3.5-turbo"
                }
            },
            "properties": [
                {
                    "name": "content",
                    "dataType": ["text"],
                    "description": "Full document content"
                },
                {
                    "name": "title",
                    "dataType": ["string"],
                    "description": "Document title"
                },
                {
                    "name": "author",
                    "dataType": ["string"],
                    "description": "Document author"
                },
                {
                    "name": "document_type",
                    "dataType": ["string"],
                    "description": "Type of document (pdf, docx, etc.)"
                },
                {
                    "name": "language",
                    "dataType": ["string"],
                    "description": "Detected language"
                },
                {
                    "name": "created_date",
                    "dataType": ["date"],
                    "description": "Document creation date"
                },
                {
                    "name": "file_size",
                    "dataType": ["int"],
                    "description": "File size in bytes"
                },
                {
                    "name": "tags",
                    "dataType": ["string[]"],
                    "description": "Document tags"
                },
                {
                    "name": "user_id",
                    "dataType": ["string"],
                    "description": "Owner user ID"
                },
                {
                    "name": "postgres_id",
                    "dataType": ["string"],
                    "description": "Reference to PostgreSQL record"
                }
            ]
        },
        {
            "class": "DocumentChunk",
            "description": "Individual chunks of larger documents",
            "vectorizer": "text2vec-transformers",
            "moduleConfig": {
                "text2vec-transformers": {
                    "poolingStrategy": "masked_mean",
                    "model": "sentence-transformers/all-MiniLM-L6-v2"
                }
            },
            "properties": [
                {
                    "name": "content",
                    "dataType": ["text"],
                    "description": "Chunk content"
                },
                {
                    "name": "chunk_index",
                    "dataType": ["int"],
                    "description": "Index within document"
                },
                {
                    "name": "start_position",
                    "dataType": ["int"],
                    "description": "Start character position"
                },
                {
                    "name": "end_position",
                    "dataType": ["int"],
                    "description": "End character position"
                },
                {
                    "name": "confidence_score",
                    "dataType": ["number"],
                    "description": "Extraction confidence"
                },
                {
                    "name": "parent_document",
                    "dataType": ["Document"],
                    "description": "Reference to parent document"
                },
                {
                    "name": "postgres_id",
                    "dataType": ["string"],
                    "description": "Reference to PostgreSQL record"
                }
            ]
        },
        {
            "class": "AudioTranscript",
            "description": "Transcribed audio content with speaker information",
            "vectorizer": "text2vec-transformers",
            "properties": [
                {
                    "name": "transcript",
                    "dataType": ["text"],
                    "description": "Full transcript text"
                },
                {
                    "name": "language",
                    "dataType": ["string"],
                    "description": "Detected language"
                },
                {
                    "name": "confidence",
                    "dataType": ["number"],
                    "description": "Transcription confidence"
                },
                {
                    "name": "speakers",
                    "dataType": ["string[]"],
                    "description": "Identified speakers"
                },
                {
                    "name": "duration",
                    "dataType": ["number"],
                    "description": "Audio duration in seconds"
                },
                {
                    "name": "timestamps",
                    "dataType": ["string"],
                    "description": "JSON with word-level timestamps"
                },
                {
                    "name": "parent_document",
                    "dataType": ["Document"],
                    "description": "Reference to source audio file"
                }
            ]
        },
        {
            "class": "ImageEmbedding",
            "description": "Visual features and metadata for images",
            "vectorizer": "img2vec-neural",
            "moduleConfig": {
                "img2vec-neural": {
                    "imageFields": ["image"],
                    "model": "resnet50"
                }
            },
            "properties": [
                {
                    "name": "image",
                    "dataType": ["blob"],
                    "description": "Base64 encoded image"
                },
                {
                    "name": "description",
                    "dataType": ["text"],
                    "description": "Image description/caption"
                },
                {
                    "name": "width",
                    "dataType": ["int"],
                    "description": "Image width in pixels"
                },
                {
                    "name": "height",
                    "dataType": ["int"],
                    "description": "Image height in pixels"
                },
                {
                    "name": "format",
                    "dataType": ["string"],
                    "description": "Image format (jpg, png, etc.)"
                },
                {
                    "name": "dominant_colors",
                    "dataType": ["string[]"],
                    "description": "Dominant colors in image"
                },
                {
                    "name": "objects_detected",
                    "dataType": ["string[]"],
                    "description": "Detected objects in image"
                },
                {
                    "name": "parent_document",
                    "dataType": ["Document"],
                    "description": "Reference to source document"
                }
            ]
        }
    ]
}
```

### 🔍 Advanced Search Capabilities

```python
class AdvancedSearchService:
    """
    Zaawansowane możliwości wyszukiwania w Weaviate
    """
    
    def semantic_search(self, query: str, filters: dict = None, limit: int = 10):
        """Wyszukiwanie semantyczne z filtrami"""
        where_filter = self._build_where_filter(filters)
        
        result = (
            self.client.query
            .get("DocumentChunk", [
                "content", "chunk_index", "confidence_score",
                "parent_document { ... on Document { title, author, document_type } }"
            ])
            .with_near_text({
                "concepts": [query],
                "distance": 0.7
            })
            .with_where(where_filter)
            .with_limit(limit)
            .with_additional(["distance", "certainty"])
            .do()
        )
        
        return self._process_search_results(result)
    
    def hybrid_search(self, query: str, alpha: float = 0.5, limit: int = 10):
        """Hybrydowe wyszukiwanie (semantyczne + BM25)"""
        result = (
            self.client.query
            .get("DocumentChunk", [
                "content", "chunk_index",
                "parent_document { ... on Document { title, author } }"
            ])
            .with_hybrid(query, alpha=alpha)
            .with_limit(limit)
            .with_additional(["score"])
            .do()
        )
        
        return self._process_search_results(result)
    
    def multi_modal_search(self, text_query: str = None, image_query: str = None):
        """Wyszukiwanie multimodalne (tekst + obraz)"""
        concepts = []
        if text_query:
            concepts.append({"text": text_query})
        if image_query:
            concepts.append({"image": image_query})
            
        result = (
            self.client.query
            .get("Document", ["title", "content", "document_type"])
            .with_near_text({"concepts": concepts})
            .with_limit(20)
            .do()
        )
        
        return result
    
    def find_similar_documents(self, document_id: str, limit: int = 5):
        """Znajdź podobne dokumenty"""
        # Pobierz wektor dokumentu źródłowego
        source_doc = self.client.query.get("Document", ["_additional { vector }"]).with_where({
            "path": ["postgres_id"],
            "operator": "Equal",
            "valueString": document_id
        }).do()
        
        if not source_doc["data"]["Get"]["Document"]:
            return []
            
        source_vector = source_doc["data"]["Get"]["Document"][0]["_additional"]["vector"]
        
        # Znajdź podobne dokumenty
        result = (
            self.client.query
            .get("Document", ["title", "author", "document_type", "postgres_id"])
            .with_near_vector({
                "vector": source_vector,
                "distance": 0.8
            })
            .with_limit(limit + 1)  # +1 bo źródłowy dokument też będzie w wynikach
            .with_additional(["distance"])
            .do()
        )
        
        # Usuń źródłowy dokument z wyników
        documents = result["data"]["Get"]["Document"]
        return [doc for doc in documents if doc["postgres_id"] != document_id]
```

---

## 🚀 Konfiguracja i Deployment

### 🐳 Docker Configuration

#### Production Docker Compose
```yaml
version: '3.8'

services:
  # Load Balancer
  nginx:
    image: nginx:1.25-alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf:ro
      - ./nginx/sites-enabled:/etc/nginx/sites-enabled:ro
      - ./ssl:/etc/nginx/ssl:ro
      - ./logs/nginx:/var/log/nginx
    depends_on:
      - app1
      - app2
      - app3
    restart: unless-stopped
    networks:
      - frontend
      - backend
    deploy:
      resources:
        limits:
          memory: 512M
        reservations:
          memory: 256M

  # FastAPI Applications
  app1: &app
    build:
      context: .
      dockerfile: Dockerfile
      target: production
    environment: &app_env
      DATABASE_URL: postgresql://postgres:${POSTGRES_PASSWORD}@postgres-primary:5432/${POSTGRES_DB}
      DATABASE_REPLICA_URL: postgresql://postgres:${POSTGRES_PASSWORD}@postgres-replica:5432/${POSTGRES_DB}
      CELERY_BROKER_URL: redis://redis-cluster:6379/0
      CELERY_RESULT_BACKEND: redis://redis-cluster:6379/1
      WEAVIATE_URL: http://weaviate:8080
      WEAVIATE_API_KEY: ${WEAVIATE_API_KEY}
      JWT_SECRET_KEY: ${JWT_SECRET_KEY}
      ENVIRONMENT: production
      LOG_LEVEL: INFO
      SENTRY_DSN: ${SENTRY_DSN}
      S3_BUCKET: ${S3_BUCKET}
      S3_ACCESS_KEY: ${S3_ACCESS_KEY}
      S3_SECRET_KEY: ${S3_SECRET_KEY}
      S3_ENDPOINT: ${S3_ENDPOINT}
      OPENAI_API_KEY: ${OPENAI_API_KEY}
    volumes:
      - uploads:/app/uploads
      - logs:/app/logs
    depends_on:
      postgres-primary:
        condition: service_healthy
      redis-cluster:
        condition: service_healthy
      weaviate:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - backend
    deploy:
      resources:
        limits:
          memory: 2G
          cpus: '1.0'
        reservations:
          memory: 1G
          cpus: '0.5'
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 40s

  app2:
    <<: *app
    
  app3:
    <<: *app

  # Celery Workers
  worker-default: &worker
    build:
      context: .
      dockerfile: Dockerfile.worker
      target: production
    command: celery -A app.celery_app worker --loglevel=info -Q default --concurrency=4 --prefetch-multiplier=1
    environment: *app_env
    volumes:
      - uploads:/app/uploads
      - logs:/app/logs
      - worker-temp:/tmp
    depends_on:
      postgres-primary:
        condition: service_healthy
      redis-cluster:
        condition: service_healthy
      weaviate:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - backend
    deploy:
      replicas: 3
      resources:
        limits:
          memory: 4G
          cpus: '2.0'
        reservations:
          memory: 2G
          cpus: '1.0'
    healthcheck:
      test: ["CMD", "celery", "-A", "app.celery_app", "inspect", "ping"]
      interval: 60s
      timeout: 10s
      retries: 3

  worker-high-priority:
    <<: *worker
    command: celery -A app.celery_app worker --loglevel=info -Q high_priority --concurrency=2
    deploy:
      replicas: 2
      resources:
        limits:
          memory: 8G
          cpus: '4.0'
        reservations:
          memory: 4G
          cpus: '2.0'

  worker-low-priority:
    <<: *worker
    command: celery -A app.celery_app worker --loglevel=info -Q low_priority --concurrency=8
    deploy:
      replicas: 2
      resources:
        limits:
          memory: 6G
          cpus: '3.0'
        reservations:
          memory: 3G
          cpus: '1.5'

  # Celery Beat Scheduler
  celery-beat:
    build:
      context: .
      dockerfile: Dockerfile.worker
    command: celery -A app.celery_app beat --loglevel=info --scheduler django_celery_beat.schedulers:DatabaseScheduler
    environment: *app_env
    volumes:
      - logs:/app/logs
    depends_on:
      postgres-primary:
        condition: service_healthy
      redis-cluster:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - backend

  # Celery Flower (Monitoring)
  flower:
    build:
      context: .
      dockerfile: Dockerfile.worker
    command: celery -A app.celery_app flower --port=5555
    ports:
      - "5555:5555"
    environment: *app_env
    depends_on:
      - redis-cluster
    restart: unless-stopped
    networks:
      - backend
      - monitoring

  # Database Primary
  postgres-primary:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_REPLICATION_USER: replicator
      POSTGRES_REPLICATION_PASSWORD: ${POSTGRES_REPLICATION_PASSWORD}
    ports:
      - "5432:5432"
    volumes:
      - postgres_primary_data:/var/lib/postgresql/data
      - ./scripts/postgres/init:/docker-entrypoint-initdb.d
      - ./postgres/postgresql.conf:/etc/postgresql/postgresql.conf
      - postgres_logs:/var/log/postgresql
    command: |
      postgres
        -c config_file=/etc/postgresql/postgresql.conf
        -c log_destination=stderr
        -c logging_collector=on
        -c log_directory=/var/log/postgresql
        -c log_filename=postgresql-%Y-%m-%d.log
    restart: unless-stopped
    networks:
      - backend
    deploy:
      resources:
        limits:
          memory: 4G
          cpus: '2.0'
        reservations:
          memory: 2G
          cpus: '1.0'
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres -d ${POSTGRES_DB}"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Database Replica (Read-only)
  postgres-replica:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      PGUSER: postgres
    volumes:
      - postgres_replica_data:/var/lib/postgresql/data
      - ./scripts/postgres/replica:/docker-entrypoint-initdb.d
    command: |
      postgres
        -c wal_level=replica
        -c max_wal_senders=3
        -c wal_keep_segments=64
    depends_on:
      postgres-primary:
        condition: service_healthy
    restart: unless-stopped
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres -d ${POSTGRES_DB}"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Redis Cluster
  redis-cluster:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    command: |
      redis-server
        --appendonly yes
        --maxmemory 2gb
        --maxmemory-policy allkeys-lru
        --save 900 1
        --save 300 10
        --save 60 10000
    volumes:
      - redis_data:/data
      - ./redis/redis.conf:/etc/redis/redis.conf
    restart: unless-stopped
    networks:
      - backend
    deploy:
      resources:
        limits:
          memory: 2G
          cpus: '1.0'
        reservations:
          memory: 1G
          cpus: '0.5'
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Cache Redis (Separate instance)
  redis-cache:
    image: redis:7-alpine
    ports:
      - "6380:6379"
    command: |
      redis-server
        --maxmemory 1gb
        --maxmemory-policy allkeys-lru
        --save ""
    volumes:
      - redis_cache_data:/data
    restart: unless-stopped
    networks:
      - backend
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 30s
      timeout: 10s
      retries: 3

  # Weaviate Vector Database
  weaviate:
    image: semitechnologies/weaviate:1.22.4
    ports:
      - "8080:8080"
    environment:
      QUERY_DEFAULTS_LIMIT: 25
      AUTHENTICATION_ANONYMOUS_ACCESS_ENABLED: 'false'
      AUTHENTICATION_APIKEY_ENABLED: 'true'
      AUTHENTICATION_APIKEY_ALLOWED_KEYS: ${WEAVIATE_API_KEY}
      AUTHENTICATION_APIKEY_USERS: 'admin'
      PERSISTENCE_DATA_PATH: '/var/lib/weaviate'
      DEFAULT_VECTORIZER_MODULE: 'text2vec-transformers'
      ENABLE_MODULES: 'text2vec-transformers,text2vec-openai,generative-openai,img2vec-neural'
      TRANSFORMERS_INFERENCE_API: 'http://t2v-transformers:8080'
      CLUSTER_HOSTNAME: 'node1'
      CLUSTER_GOSSIP_BIND_PORT: '7100'
      CLUSTER_DATA_BIND_PORT: '7101'
    volumes
