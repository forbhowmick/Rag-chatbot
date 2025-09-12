# Environment Setup for RAG Chatbot with Google Gemini

## Quick Setup Using .env File (Recommended)

1. **Copy the example file:**
   ```
   Copy .env.example to .env
   ```

2. **Get your Gemini API Key** (Required for AI functionality):
   - Go to [Google AI Studio](https://aistudio.google.com/)
   - Sign in with your Google account
   - Click "Get API Key" in the left sidebar
   - Create a new API key
   - Copy the generated API key

3. **Edit your .env file:**
   ```
   GEMINI_API_KEY=your-gemini-api-key-here
   GEMINI_MODEL=gemini-1.5-flash
   ```

5. **Save the file** - The application will automatically load these variables on startup.

## Alternative: PowerShell Environment Variables

If you prefer not to use a .env file:
```powershell
$env:GOOGLE_API_KEY="your-google-api-key-here"
$env:GEMINI_MODEL="gemini-1.5-flash"
```

## Benefits of Using Gemini Flash

- **Most Cost-Effective**: Gemini 1.5 Flash is Google's cheapest model option
- **High Performance**: Fast response times with excellent quality
- **Single API Key**: Use one Google API key for both Drive access and AI functionality
- **Google Ecosystem**: Perfect integration with Google Drive and Docs
- **Advanced Capabilities**: Latest Gemini technology at the lowest cost

## Model Options (in order of cost, lowest to highest):

1. **gemini-1.5-flash** (Recommended) - Cheapest, fastest, great for most use cases
2. **gemini-1.5-pro** - Higher quality, more expensive
3. **gemini-pro** - Legacy model, mid-range pricing

## Required APIs to Enable in Google Cloud Console

1. Google Drive API
2. Google Docs API
3. Google Sheets API (optional, for bonus features)

## OAuth Setup Requirements

1. Configure OAuth consent screen
2. Create OAuth 2.0 Client ID
3. Download client_secret.json and place in backend/ folder
4. Add authorized redirect URIs: http://localhost:5000/oauth2callback

## Testing the Setup

After setting up the API key, restart the Flask application and try the OAuth flow again.
