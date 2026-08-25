# 🏮 LatenTech Backend

### Backend API for LatenTech & LaternChat

**LatenTech Backend** is a lightweight Node.js backend service built with **Express.js** to support the LatenTech platform and **LaternChat** AI experience.

The backend provides API endpoints for file processing, PDF text extraction, AI chat requests, health monitoring, and frontend integration.

---

## ✨ Features

### 🤖 AI Chat API

Provides a backend proxy for AI chat requests through the **NVIDIA API**.

The `/api/chat` endpoint accepts model configuration and chat messages, forwards the request to NVIDIA's chat completion API, and returns the generated response.

### 📄 PDF & File Processing

The backend supports file uploads through `multipart/form-data`.

The `/api/upload` endpoint can:

* Accept uploaded files
* Detect PDF files
* Extract text from PDFs
* Read non-PDF files as text
* Return extracted content and the original filename

### ❤️ Health Check

A simple health endpoint is available for monitoring backend availability:

```http
GET /api/health
```

Example response:

```json
{
  "status": "ok",
  "message": "Backend is running"
}
```

### 🌐 CORS Support

CORS middleware is configured to allow frontend applications to communicate with the backend API.

### ⚙️ Environment Configuration

The backend uses `dotenv` for environment-based configuration.

---

## 🛠️ Technology Stack

| Technology              | Purpose                   |
| ----------------------- | ------------------------- |
| Node.js                 | Runtime environment       |
| Express.js              | Backend API framework     |
| JavaScript / ES Modules | Application development   |
| Multer                  | File upload handling      |
| pdf-parse               | PDF text extraction       |
| CORS                    | Cross-origin API access   |
| dotenv                  | Environment configuration |
| NVIDIA API              | AI chat processing        |

The repository's `package.json` defines these dependencies and requires Node.js 18 or newer.

---

## 🏗️ Architecture

```text
                    LatenTech Frontend
                           │
                           ▼
                  ┌──────────────────┐
                  │   LatenTech API  │
                  │    Express.js    │
                  └────────┬─────────┘
                           │
             ┌─────────────┼─────────────┐
             │             │             │
             ▼             ▼             ▼
         File Upload    AI Chat      Health Check
             │             │
             ▼             ▼
         PDF Parser     NVIDIA API
```

---

## 📁 Project Structure

```text
latentech_backend/
│
├── api/
│   └── index.js
│
├── package.json
├── package-lock.json
├── .env
├── .gitignore
└── README.md
```

The application entry point is `api/index.js`, as specified by the project's package configuration.

---

## 🚀 Getting Started

### Prerequisites

Make sure you have:

* Node.js 18+
* npm
* Git
* NVIDIA API credentials for AI chat functionality

Check your Node.js installation:

```bash
node --version
```

---

## 📥 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/gow1n-d/latentech_backend.git
```

### 2. Navigate to the Project

```bash
cd latentech_backend
```

### 3. Install Dependencies

```bash
npm install
```

---

## 🔐 Environment Variables

Create a `.env` file in the project root:

```env
PORT=3000
NVIDIA_API_KEY=your_nvidia_api_key
```

The backend reads configuration values from environment variables using `dotenv`.

### Important

Never commit your real API key to GitHub.

Add this to `.gitignore`:

```gitignore
.env
node_modules/
```

---

## ▶️ Run the Server

Start the backend with:

```bash
npm start
```

The server will run on:

```text
http://localhost:3000
```

The start script and application entry point are defined in `package.json`.

---

## 🔌 API Documentation

### 1. Health Check

```http
GET /api/health
```

Example:

```bash
curl http://localhost:3000/api/health
```

Response:

```json
{
  "status": "ok",
  "message": "Backend is running"
}
```

---

### 2. Upload File

```http
POST /api/upload
```

Upload a file using the `file` form field.

Example:

```bash
curl -X POST \
  -F "file=@document.pdf" \
  http://localhost:3000/api/upload
```

Example response:

```json
{
  "text": "Extracted document text...",
  "filename": "document.pdf"
}
```

PDF files are parsed using `pdf-parse`; non-PDF files are processed as UTF-8 text.

---

### 3. AI Chat

```http
POST /api/chat
```

Example request:

```json
{
  "model": "your-model",
  "messages": [
    {
      "role": "user",
      "content": "Explain artificial intelligence."
    }
  ],
  "temperature": 0.7,
  "max_tokens": 500,
  "stream": false
}
```

The backend forwards these parameters to NVIDIA's chat completion API.

---

## 🔄 Request Flow

### AI Chat

```text
User
 │
 ▼
LatenTech Frontend
 │
 ▼
POST /api/chat
 │
 ▼
Express Backend
 │
 ▼
NVIDIA AI API
 │
 ▼
AI Response
 │
 ▼
Frontend
```

### Document Processing

```text
User
 │
 ▼
Upload PDF / Text File
 │
 ▼
POST /api/upload
 │
 ▼
Multer
 │
 ▼
PDF Parser / Text Reader
 │
 ▼
Extracted Text
 │
 ▼
Frontend / LaternChat
```

---

## 🛡️ Error Handling

The backend returns appropriate HTTP error responses for common problems.

### Missing File

```json
{
  "error": "No file uploaded"
}
```

### Server Error

```json
{
  "error": "Error message"
}
```

### AI API Error

Errors returned by NVIDIA are forwarded to the client with the corresponding HTTP status when possible.

---

## 🔐 Security Recommendations

Before using this backend in production:

* Keep API credentials only in environment variables.
* Rotate any credentials that have ever been committed to source control.
* Restrict CORS to trusted frontend domains.
* Add authentication and authorization.
* Validate uploaded file types.
* Limit upload size.
* Rate-limit public API endpoints.
* Sanitize user input.
* Add request logging and monitoring.
* Use HTTPS in production.

---

## 🌍 Frontend Integration

This backend can be connected to the LatenTech frontend and LaternChat interfaces.

Example frontend request:

```javascript
const response = await fetch(
  "https://your-backend-domain.com/api/chat",
  {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      model: "your-model",
      messages: [
        {
          role: "user",
          content: "Hello!"
        }
      ],
      temperature: 0.7,
      max_tokens: 500,
      stream: false
    })
  }
);

const data = await response.json();
```

---

## 🚢 Deployment

The application can be deployed on platforms supporting Node.js, such as:

* Render
* Railway
* AWS
* Google Cloud
* Azure
* DigitalOcean
* Fly.io
* VPS / Docker environments

Set the production environment variables before starting the application:

```env
PORT=3000
NVIDIA_API_KEY=your_production_key
```

Then:

```bash
npm install
npm start
```

---

## 🔮 Future Enhancements

Planned or possible improvements include:

* 🔐 JWT authentication
* 👥 User accounts and roles
* 💬 Conversation history
* 🧠 Multiple AI model support
* 📚 Document-aware AI chat
* 📑 DOCX support
* 🖼️ Image/document understanding
* 📊 Usage analytics
* 🔍 Semantic document search
* 🗄️ Database integration
* 🧾 API documentation with Swagger
* 🚦 Rate limiting
* 🐳 Docker support
* 🔄 CI/CD automation
* 📈 Production monitoring

---

## 🤝 Contributing

Contributions are welcome.

Create a feature branch:

```bash
git checkout -b feature/new-feature
```

Make your changes:

```bash
git add .
git commit -m "Add new feature"
```

Push the branch:

```bash
git push origin feature/new-feature
```

Then create a Pull Request.

---

## 📄 License

This project currently does not specify a license.

---

## 👨‍💻 Author

**gow1n-d**

GitHub:

https://github.com/gow1n-d

Repository:

https://github.com/gow1n-d/latentech_backend

---

## ⭐ Support

If you find this project useful, consider giving the repository a ⭐ on GitHub.

---

### 🏮 LatenTech Backend

**Powering LatenTech and LaternChat with reliable APIs and AI integrations.**
