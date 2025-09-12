# 🤖 RAG Document Assistant

> Transform your Google Drive documents into an intelligent, AI-powered knowledge base

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-2.0+-green.svg)](https://flask.palletsprojects.com/)
[![Google OAuth 2.0](https://img.shields.io/badge/OAuth-2.0-red.svg)](https://developers.google.com/identity/protocols/oauth2)
[![Gemini AI](https://img.shields.io/badge/Gemini-AI-purple.svg)](https://ai.google.dev/)

## 🌟 Overview

The RAG Document Assistant is a sophisticated Flask-based web application that leverages Retrieval-Augmented Generation (RAG) technology to create an intelligent knowledge base from your Google Drive documents. Using Google Gemini AI for cost-effective processing, it enables natural conversation with your documents through an intuitive chat interface.

### ✨ Key Features

- **🔐 Secure Authentication**: Google OAuth 2.0 integration with minimal required permissions
- **📁 Multi-Format Support**: Google Docs, Sheets, Slides, PDFs, PowerPoint, and text files
- **🧠 AI-Powered RAG**: Google Gemini AI for embeddings and natural language processing
- **⚡ Fast Search**: FAISS vector database for efficient similarity search
- **💬 Real-Time Chat**: Modern, responsive chat interface with typing indicators
- **🎨 Beautiful UI**: Professional design with animations and responsive layout
- **🔒 Privacy-First**: Documents processed locally, never sent to external services
- **💰 Cost-Optimized**: Uses Gemini 1.5 Flash for affordable AI processing

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- Google Cloud Project with Drive and Docs API enabled
- Google Gemini API key

### 1. Clone and Setup

```bash
git clone https://github.com/yourusername/rag-document-assistant.git
cd rag-document-assistant

# Create virtual environment (recommended)
python -m venv .venv
.venv\Scripts\activate  # On Windows

# Install dependencies
pip install -r requirements.txt
```

### 2. Google Cloud Configuration

1. **Create Google Cloud Project**:
   - Visit [Google Cloud Console](https://console.cloud.google.com/)
   - Create a new project or select existing one

2. **Enable Required APIs**:
   - Navigate to "APIs & Services" → "Library"
   - Enable "Google Drive API"
   - Enable "Google Docs API"

3. **Setup OAuth 2.0**:
   - Navigate to "APIs & Services" → "Credentials"
   - Click "Create Credentials" → "OAuth 2.0 Client IDs"
   - Application type: "Web application"
   - Authorized redirect URIs: `http://localhost:5000/oauth2callback`
   - Download JSON and save as `backend/client_secret.json`

### 3. Environment Configuration

Create a `.env` file in the project root:

```env
# Required: Google Gemini API Key
GEMINI_API_KEY=your_gemini_api_key_here

# Optional: Model selection (default: gemini-1.5-flash)
GEMINI_MODEL=gemini-1.5-flash

# Development only (remove in production)
OAUTHLIB_INSECURE_TRANSPORT=1
```

### 4. Launch Application

```powershell
# Create logs directory
mkdir backend\logs

# Start the application
python backend\app.py
```

Visit `http://localhost:5000` to access the application.

## 📖 User Guide

### Getting Started

1. **🔑 Authentication**
   - Click "Continue with Google" on the landing page
   - Grant permissions for Drive and Docs access
   - You'll be redirected to document selection

2. **📄 Document Selection**
   - Browse your Google Drive documents
   - Select documents to include in your knowledge base
   - Supported formats are clearly indicated
   - Click "Continue to Chat" when ready

3. **💬 Intelligent Chat**
   - Ask questions about your documents
   - Get AI-powered answers with source attribution
   - Enjoy real-time responses with typing indicators

### Supported Document Types

| Format | File Extension | Description |
|--------|---------------|-------------|
| Google Docs | `.gdoc` | Native Google Documents |
| Google Sheets | `.gsheet` | Spreadsheets with text extraction |
| Google Slides | `.gslides` | Presentations with slide text |
| PDF Documents | `.pdf` | Portable Document Format |
| PowerPoint | `.ppt`, `.pptx` | Microsoft PowerPoint presentations |
| Text Files | `.txt` | Plain text documents |

## 🏗️ Architecture

### Technology Stack

**Backend**:
- **Flask**: Web framework and routing
- **Google APIs**: Drive and Docs integration
- **LangChain**: RAG pipeline framework
- **FAISS**: Vector similarity search
- **Gemini AI**: Embeddings and language model

**Frontend**:
- **HTML5/CSS3**: Modern web standards
- **JavaScript (jQuery)**: Dynamic interactions
- **Font Awesome**: Professional iconography
- **Responsive Design**: Mobile-first approach

## 📂 Project Structure

```
RAG-Document-Assistant/
├── 📁 backend/
│   ├── 🐍 app.py                    # Main Flask application
│   ├── 🔑 client_secret.json        # OAuth credentials (create this)
│   └── 📁 logs/
│       └── 📄 app.log              # Application logs
├── 📁 frontend/
│   └── 📁 templates/
│       ├── 🏠 index.html           # Landing page
│       ├── 📄 docs.html            # Document selection
│       └── 💬 chat.html            # Chat interface
├── 🌍 .env                         # Environment variables (create this)
├── 📋 requirements.txt             # Python dependencies
├── 📚 README.md                    # This file
├── 🏗️ ARCHITECTURE.md              # System architecture documentation
├── 📖 API_DOCUMENTATION.md         # Comprehensive API reference
└── 💻 CODE_DOCUMENTATION.md        # Detailed code analysis
```

## ⚙️ Configuration

### Environment Variables

| Variable | Description | Default | Required |
|----------|-------------|---------|----------|
| `GEMINI_API_KEY` | Google Gemini API key | None | ✅ Yes |
| `GEMINI_MODEL` | Gemini model selection | `gemini-1.5-flash` | ❌ No |
| `OAUTHLIB_INSECURE_TRANSPORT` | Allow HTTP OAuth (dev only) | `0` | ❌ No |

## 🔒 Security & Privacy

### Security Features

- **OAuth 2.0**: Industry-standard authentication
- **Minimal Scopes**: Read-only access to necessary resources
- **CSRF Protection**: State parameter validation
- **Session Security**: Server-side session management
- **Input Sanitization**: Query and parameter validation

### Privacy Guarantees

- **Local Processing**: Documents processed on your server
- **No External Sharing**: Content never sent to third parties
- **Session Isolation**: User data completely separated
- **Audit Logging**: Comprehensive activity tracking

## 🚀 Deployment

### Development Deployment

```powershell
# Using Flask development server
python backend\app.py
```

### Production Deployment

```powershell
# Using Gunicorn (recommended)
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:8000 backend.app:app
```

## 📊 Performance & Scalability

### Current Limitations

- **Memory Storage**: Vector store and documents in RAM
- **Single User Sessions**: File-based session storage
- **Sequential Processing**: Documents processed one by one

### Scaling Recommendations

- **Database Integration**: PostgreSQL with pgvector
- **Redis Sessions**: Distributed session management
- **Async Processing**: Celery for background tasks
- **Load Balancing**: Multiple application instances

## 🆘 Troubleshooting

### Common Issues

**OAuth Errors**:
```powershell
# Check client_secret.json path and content
# Verify redirect URI in Google Cloud Console
# Ensure APIs are enabled
```

**API Key Issues**:
```powershell
# Verify Gemini API key in .env file
# Check API quotas in Google Cloud Console
# Ensure billing is enabled
```

**Document Processing Errors**:
```powershell
# Check document permissions in Google Drive
# Verify supported document formats
# Review application logs for specific errors
```

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes with proper testing
4. Submit a pull request with detailed description

## 📋 Changelog

### v1.0.0 (Current)
- ✅ Google OAuth 2.0 authentication
- ✅ Multi-format document support (Google Docs, Sheets, Slides, PDF, PowerPoint)
- ✅ Google Gemini AI integration
- ✅ Modern responsive UI with animations
- ✅ RAG pipeline with FAISS vector search
- ✅ Comprehensive documentation suite
- ✅ Production-ready code cleanup

### Planned Features (v1.1.0)
- 🔄 Database persistence
- 🔄 Multi-user support
- 🔄 Advanced query features
- 🔄 Document versioning
- 🔄 API rate limiting

## 📞 Support

- **Documentation**: Check our comprehensive docs in the repository
- **Issues**: Open a GitHub issue for bugs or feature requests
- **Architecture**: See [ARCHITECTURE.md](ARCHITECTURE.md) for system design
- **API Reference**: See [API_DOCUMENTATION.md](API_DOCUMENTATION.md) for API details
- **Code Analysis**: See [CODE_DOCUMENTATION.md](CODE_DOCUMENTATION.md) for implementation details

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Google**: For OAuth 2.0, Drive API, Docs API, and Gemini AI
- **LangChain**: For the excellent RAG framework
- **FAISS**: For efficient vector similarity search
- **Flask**: For the lightweight and powerful web framework

---

<div align="center">
  <p><strong>Made with ❤️ for document intelligence</strong></p>
  <p>
    <a href="#-overview">Back to Top</a> •
    <a href="ARCHITECTURE.md">Architecture</a> •
    <a href="API_DOCUMENTATION.md">API Docs</a> •
    <a href="CODE_DOCUMENTATION.md">Code Docs</a>
  </p>
</div>

## Features

- Google OAuth authentication
- Fetch and display Google Docs
- Select documents for knowledge base
- RAG-based Q&A with fallback to general knowledge
- Web-based chat interface

## Architecture

See `architecture.md` for detailed architecture overview.
