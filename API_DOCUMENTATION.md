# RAG Document Assistant - API Documentation

## 📚 API Reference

This document provides comprehensive documentation for all API endpoints in the RAG Document Assistant application.

## 🔗 Base URL

```
http://localhost:5000  # Development
https://your-domain.com  # Production
```

## 🛡️ Authentication

The application uses Google OAuth 2.0 for authentication. Most endpoints require an authenticated session.

### Authentication Flow
1. **Login**: User initiates OAuth flow
2. **Authorization**: User grants permissions on Google
3. **Callback**: Application receives authorization code
4. **Session**: Server stores credentials in session

## 📋 API Endpoints

### 🏠 Landing Page

#### `GET /`
**Description**: Serves the main landing page

**Authentication**: None required

**Response**: HTML template (`index.html`)

**Features**:
- Modern responsive design
- Feature overview
- Google OAuth login button
- Supported document types display

---

### 🔐 Authentication Endpoints

#### `GET /login`
**Description**: Initiates Google OAuth 2.0 authentication flow

**Authentication**: None required

**Parameters**: None

**Response**: Redirects to Google OAuth authorization URL

**Process**:
1. Creates OAuth flow with required scopes
2. Generates authorization URL
3. Redirects user to Google for authentication

**Scopes Required**:
- `https://www.googleapis.com/auth/drive.readonly`
- `https://www.googleapis.com/auth/documents.readonly`

---

#### `GET /oauth2callback`
**Description**: Handles OAuth callback from Google

**Authentication**: OAuth state verification

**Parameters**:
- `code` (query): Authorization code from Google
- `state` (query): CSRF protection state

**Response**: 
- **Success**: Redirects to `/docs`
- **Error**: Error message with details

**Process**:
1. Exchanges authorization code for access token
2. Stores credentials in server-side session
3. Redirects to document selection page

**Error Handling**:
- Invalid state parameter
- Authorization code exchange failure
- Credential serialization errors

---

#### `GET /logout`
**Description**: Clears user session and logs out

**Authentication**: None required

**Response**: Redirects to `/`

**Process**:
1. Clears all session data
2. Invalidates user authentication
3. Redirects to landing page

---

### 📄 Document Management Endpoints

#### `GET /docs`
**Description**: Displays document selection interface with user's Google Drive files

**Authentication**: Required (OAuth session)

**Response**: HTML template (`docs.html`) with document list

**Data Provided**:
```json
{
  "docs": [
    {
      "id": "document_id",
      "name": "Document Name",
      "mimeType": "application/vnd.google-apps.document"
    }
  ],
  "no_docs": false
}
```

**Supported Document Types**:
- Google Docs (`application/vnd.google-apps.document`)
- Google Sheets (`application/vnd.google-apps.spreadsheet`)
- Google Slides (`application/vnd.google-apps.presentation`)
- PDF Files (`application/pdf`)
- PowerPoint (`application/vnd.openxmlformats-officedocument.presentationml.presentation`)
- Legacy PowerPoint (`application/vnd.ms-powerpoint`)
- Plain Text (`text/plain`)

**Process**:
1. Validates user authentication
2. Queries Google Drive API for compatible documents
3. Filters by supported MIME types
4. Returns paginated results (max 100 documents)

**Error Handling**:
- Session timeout/invalid credentials → Redirect to login
- API quota exceeded → Error message
- Network connectivity issues → Retry mechanism

---

#### `POST /select_docs`
**Description**: Processes selected documents and creates vector store

**Authentication**: Required (OAuth session)

**Request Body**:
```form-data
doc_ids[]: document_id_1
doc_ids[]: document_id_2
```

**Response**: Redirects to `/chat`

**Process**:
1. **Document Retrieval**: Fetches selected documents from Google Drive
2. **Content Extraction**: 
   - Google Docs → Google Docs API
   - Google Sheets → Export as text
   - Google Slides → Export as PPTX, extract text
   - PDF → PyPDF2 text extraction
   - PowerPoint → python-pptx text extraction
   - Plain Text → Direct content retrieval
3. **Text Processing**:
   - Document chunking (1000 character chunks)
   - Metadata preservation (source, name, type)
4. **Vector Store Creation**:
   - Generate embeddings using Google Gemini
   - Create FAISS vector index
   - Store in memory for session

**Error Handling**:
- No documents selected → Error message
- Document processing failures → Skip and continue
- Empty documents → Warning and skip
- API quota exceeded → Error message

**Performance Considerations**:
- Sequential processing (can be optimized with parallel processing)
- Memory usage scales with document count and size
- Processing time varies by document type and size

---

### 💬 Chat Interface Endpoints

#### `GET /chat`
**Description**: Displays the chat interface for RAG queries

**Authentication**: Required (OAuth session)

**Response**: HTML template (`chat.html`)

**Template Variables**:
```json
{
  "gemini_available": true,
  "selected_docs_count": 5
}
```

**Features**:
- Real-time chat interface
- System status display
- Document count indicator
- Responsive design with animations
- Typing indicators
- Error handling

---

#### `POST /query`
**Description**: Processes user queries using RAG pipeline

**Authentication**: Required (OAuth session)

**Request Headers**:
```
Content-Type: application/json
```

**Request Body**:
```json
{
  "query": "What is the main topic discussed in the documents?"
}
```

**Response**:
```json
{
  "response": "Based on your documents, the main topic discussed is..."
}
```

**Process**:
1. **Query Validation**: Ensures query is not empty
2. **Vector Search**: 
   - Generate query embedding using Google Gemini
   - Search FAISS vector store for similar documents
   - Retrieve top-k relevant chunks (default: 4)
3. **Context Preparation**:
   - Combine retrieved document chunks
   - Prepare context with source information
4. **Response Generation**:
   - Use Google Gemini with retrieved context
   - Generate contextually relevant response
   - Include source attribution when applicable
5. **Fallback Handling**:
   - If no relevant documents found → General knowledge response
   - If API unavailable → Error message

**RAG Pipeline Details**:
```python
# Similarity search parameters
similarity_threshold = 0.7
max_chunks = 4
chunk_size = 1000

# LLM parameters
temperature = 0.7
max_tokens = 1000
model = "gemini-1.5-flash"
```

**Error Handling**:
- Empty query → Validation error
- No vector store → Error message
- API quota exceeded → Fallback response
- Network timeout → Retry mechanism

---

## 🔧 Utility Functions

### Document Processing Functions

#### `extract_text_from_doc(doc)`
**Purpose**: Extracts text from Google Docs API response

**Parameters**:
- `doc` (dict): Google Docs API document object

**Returns**: String containing extracted text

**Process**:
- Iterates through document body content
- Extracts text from paragraph elements
- Handles nested text runs

---

#### `extract_text_from_pdf(pdf_data)`
**Purpose**: Extracts text from PDF binary data

**Parameters**:
- `pdf_data` (bytes): PDF file binary content

**Returns**: String containing extracted text

**Process**:
- Creates BytesIO stream from binary data
- Uses PyPDF2 to parse PDF structure
- Extracts text from all pages
- Handles encrypted/corrupted PDFs gracefully

---

#### `extract_text_from_pptx(pptx_data)`
**Purpose**: Extracts text from PowerPoint binary data

**Parameters**:
- `pptx_data` (bytes): PowerPoint file binary content

**Returns**: String containing extracted text

**Process**:
- Creates BytesIO stream from binary data
- Uses python-pptx to parse presentation
- Extracts text from slides, including:
  - Slide titles and content
  - Table data
  - Text boxes and shapes
- Organizes content by slide number

---

#### `generate_general_response(query)`
**Purpose**: Generates fallback responses using Gemini AI

**Parameters**:
- `query` (str): User's question

**Returns**: String containing AI-generated response

**Process**:
- Uses Google Gemini without document context
- Provides general knowledge response
- Maintains conversational tone

---

## 📊 Data Models

### Session Data Structure
```python
session = {
    'credentials': {
        'token': 'access_token',
        'refresh_token': 'refresh_token',
        'token_uri': 'https://oauth2.googleapis.com/token',
        'client_id': 'client_id',
        'client_secret': 'client_secret',
        'scopes': ['drive.readonly', 'documents.readonly'],
        'expiry': '2024-01-01T00:00:00Z'
    }
}
```

### Document Metadata
```python
document = {
    'id': 'google_drive_file_id',
    'name': 'Document Title.docx',
    'mimeType': 'application/vnd.google-apps.document',
    'source': 'google_drive',
    'processed_at': '2024-01-01T00:00:00Z'
}
```

### Vector Store Document
```python
langchain_document = Document(
    page_content="Document text content...",
    metadata={
        'source': 'document_id',
        'name': 'Document Title.docx',
        'chunk_index': 0,
        'total_chunks': 5
    }
)
```

## 🚨 Error Codes and Responses

### HTTP Status Codes
- `200`: Success
- `302`: Redirect (OAuth flow, navigation)
- `400`: Bad Request (invalid parameters)
- `401`: Unauthorized (invalid/expired session)
- `403`: Forbidden (insufficient permissions)
- `429`: Too Many Requests (API rate limit)
- `500`: Internal Server Error

### Error Response Format
```json
{
  "error": true,
  "message": "Human-readable error description",
  "code": "ERROR_CODE",
  "details": "Additional technical details"
}
```

### Common Error Scenarios

#### Authentication Errors
- **Session Expired**: Redirect to `/login`
- **Invalid OAuth State**: "OAuth state mismatch error"
- **Permission Denied**: "Insufficient permissions for Google Drive access"

#### Document Processing Errors
- **No Documents Selected**: "Please select at least one document"
- **Processing Failed**: "Error processing document: [document_name]"
- **Unsupported Format**: "Document type not supported: [mime_type]"

#### API Errors
- **Quota Exceeded**: "API quota exceeded. Please try again later"
- **Network Timeout**: "Request timeout. Please check your connection"
- **Service Unavailable**: "Google services temporarily unavailable"

## 🔍 Monitoring and Logging

### Log Levels
- **INFO**: Normal operations, user actions
- **WARNING**: Non-critical issues, fallback scenarios
- **ERROR**: Critical errors, exceptions
- **DEBUG**: Detailed debugging information (development only)

### Log Format
```
2024-01-01 12:00:00,000 - INFO - User authenticated successfully: user_id
2024-01-01 12:01:00,000 - WARNING - Document processing failed: document_name
2024-01-01 12:02:00,000 - ERROR - API quota exceeded for Google Drive API
```

### Monitoring Endpoints

#### Health Check (Future Enhancement)
```
GET /health
Response: {"status": "healthy", "timestamp": "2024-01-01T12:00:00Z"}
```

#### Metrics (Future Enhancement)
```
GET /metrics
Response: {"active_sessions": 5, "documents_processed": 150, "queries_served": 1000}
```

This comprehensive API documentation provides developers and system administrators with all the information needed to understand, integrate with, and maintain the RAG Document Assistant application.
