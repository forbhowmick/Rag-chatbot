# RAG Document Assistant - Architecture Documentation

## 🏗️ System Architecture Overview

The RAG Document Assistant is a Flask-based web application that enables users to create an intelligent knowledge base from their Google Drive documents using Retrieval-Augmented Generation (RAG) technology powered by Google Gemini AI.

## 🔧 Technology Stack

### Backend Technologies
- **Flask**: Python web framework for handling HTTP requests and routing
- **Google OAuth 2.0**: Authentication and authorization for Google services
- **Google Drive API**: Document retrieval and access
- **Google Docs API**: Google Docs content extraction
- **Google Gemini AI**: Language model and embeddings generation
- **LangChain**: RAG pipeline framework and document processing
- **FAISS**: Vector database for similarity search
- **PyPDF2**: PDF text extraction
- **python-pptx**: PowerPoint text extraction

### Frontend Technologies
- **HTML5**: Modern semantic markup
- **CSS3**: Advanced styling with gradients, animations, and responsive design
- **JavaScript (jQuery)**: Dynamic interactions and AJAX requests
- **Font Awesome**: Icon library for enhanced UI

### Storage & Sessions
- **Flask-Session**: Server-side session management
- **File System**: Local session storage and logging
- **In-Memory**: Vector store and document cache

## 📁 Project Structure

```
D:/Codes/OAuth/
├── backend/
│   ├── app.py                 # Main Flask application
│   ├── client_secret.json     # Google OAuth credentials
│   └── logs/
│       └── app.log           # Application logs
├── frontend/
│   └── templates/
│       ├── index.html        # Landing page
│       ├── docs.html         # Document selection interface
│       └── chat.html         # Chat interface
├── .env                      # Environment variables
├── requirements.txt          # Python dependencies
├── README.md                # Project overview
├── ARCHITECTURE.md          # This file
└── API_DOCUMENTATION.md     # API endpoints documentation
```

## 🔄 Application Flow

### 1. Authentication Flow
```
User → Landing Page → Google OAuth → Authorization → Callback → Document Selection
```

### 2. Document Processing Flow
```
Document Selection → Drive API → Content Extraction → Text Splitting → Embeddings → Vector Store
```

### 3. Query Processing Flow
```
User Query → Vector Search → Document Retrieval → Context Injection → LLM → Response
```

## 🧠 Core Components

### Authentication System
- **OAuth 2.0 Implementation**: Secure authentication with Google
- **Session Management**: Server-side session storage for user state
- **Scope Management**: Granular permissions for Drive and Docs access

### Document Processing Pipeline
- **Multi-format Support**: Google Docs, Sheets, Slides, PDF, PowerPoint, Text
- **Content Extraction**: Format-specific text extraction utilities
- **Text Chunking**: Intelligent document splitting for optimal retrieval
- **Metadata Preservation**: Document source and type information retention

### RAG System
- **Embedding Generation**: Google Gemini embedding model for semantic understanding
- **Vector Database**: FAISS for efficient similarity search
- **Retrieval System**: Context-aware document retrieval
- **Response Generation**: Google Gemini for natural language responses

### User Interface
- **Responsive Design**: Mobile-first approach with progressive enhancement
- **Real-time Chat**: AJAX-powered chat interface with typing indicators
- **Document Management**: Visual document selection with type indicators
- **Status Monitoring**: Real-time system status and configuration display

## 🔒 Security Considerations

### Authentication Security
- **OAuth 2.0**: Industry-standard authentication protocol
- **HTTPS Enforcement**: Secure transmission of sensitive data
- **Session Security**: Server-side session management with secure cookies
- **Scope Limitation**: Minimal required permissions (readonly access)

### Data Privacy
- **Local Processing**: Documents processed locally, not sent to external services
- **API Key Security**: Environment variable storage for sensitive credentials
- **Session Isolation**: User data separation through session management
- **Logging Privacy**: Sensitive information excluded from logs

## 📈 Scalability Considerations

### Current Limitations
- **In-Memory Storage**: Vector store and document cache in memory
- **Single-User Sessions**: File-based session storage
- **Synchronous Processing**: Sequential document processing

### Scaling Recommendations
- **Database Integration**: PostgreSQL with pgvector for production vector storage
- **Redis Sessions**: Distributed session management
- **Async Processing**: Celery for background document processing
- **Load Balancing**: Multiple Flask instances with shared storage

## 🎯 Performance Optimizations

### Current Optimizations
- **Efficient Embeddings**: Google Gemini's optimized embedding model
- **FAISS Indexing**: Fast vector similarity search
- **Session Caching**: User state and document metadata caching
- **Selective Processing**: Format-specific extraction methods

### Future Optimizations
- **Caching Layer**: Redis for frequently accessed documents
- **Batch Processing**: Multiple document processing in parallel
- **CDN Integration**: Static asset delivery optimization
- **Database Indexing**: Optimized queries for document metadata

## 🔧 Configuration Management

### Environment Variables
```bash
GEMINI_API_KEY=your_gemini_api_key_here
GEMINI_MODEL=gemini-1.5-flash  # Cost-optimized model selection
OAUTHLIB_INSECURE_TRANSPORT=1  # Development only
```

### Google OAuth Setup
- **Client Secrets**: `backend/client_secret.json`
- **Authorized Domains**: Configured in Google Cloud Console
- **API Scopes**: Drive readonly, Docs readonly

## 🚀 Deployment Architecture

### Development Environment
- **Local Flask Server**: Direct Python execution
- **File-based Sessions**: Local filesystem storage
- **HTTP Protocol**: Development-only insecure transport

### Production Recommendations
- **WSGI Server**: Gunicorn or uWSGI for production serving
- **Reverse Proxy**: Nginx for static files and SSL termination
- **Database**: PostgreSQL with pgvector extension
- **Monitoring**: Application performance monitoring (APM)
- **Logging**: Centralized logging with log rotation

## 🔍 Monitoring and Observability

### Current Logging
- **Application Logs**: Comprehensive logging to `backend/logs/app.log`
- **Error Tracking**: Exception logging with stack traces
- **User Actions**: Authentication and document processing events

### Monitoring Recommendations
- **Health Checks**: Endpoint monitoring for uptime
- **Performance Metrics**: Response time and throughput tracking
- **Error Rates**: Application error monitoring and alerting
- **Resource Usage**: Memory and CPU utilization tracking

## 🧪 Testing Strategy

### Current State
- **Manual Testing**: User interface and functionality testing
- **Integration Testing**: OAuth flow and API integration verification

### Testing Recommendations
- **Unit Tests**: Core function testing with pytest
- **Integration Tests**: API endpoint and OAuth flow testing
- **E2E Tests**: Selenium-based user journey testing
- **Load Testing**: Performance testing with realistic user loads

## 📊 Data Flow Diagram

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Browser   │───▶│ Flask Server │───▶│ Google APIs │
│   (Client)  │◀───│   (Backend)  │◀───│  (External) │
└─────────────┘    └──────────────┘    └─────────────┘
       │                   │                   │
       │                   ▼                   │
       │            ┌──────────────┐           │
       │            │ Session Store│           │
       │            └──────────────┘           │
       │                   │                   │
       ▼                   ▼                   ▼
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│    UI/UX    │    │ Vector Store │    │  Documents  │
│ (Templates) │    │   (FAISS)    │    │ (Drive API) │
└─────────────┘    └──────────────┘    └─────────────┘
```

This architecture provides a solid foundation for a production-ready RAG application with clear separation of concerns, security best practices, and scalability considerations.
- Requires OpenAI API key
