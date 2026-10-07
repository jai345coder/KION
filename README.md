# KION 🔺

**An agentic AI assistant that doesn't just chat — it acts.**

Kion is a full-stack AI platform built on LangChain and Google Gemini, designed around a core idea most "AI chat apps" skip: an assistant that can reason about intent, decide when to take real action, and execute it — not just generate text.

---

## 🧠 What Makes Kion Different

Most AI chat apps are a UI wrapped around a single LLM call. Kion is an **agent**:

- Holds multi-turn conversational memory across a chat
- Decides — based on context — whether to respond conversationally or call a tool
- When it calls a tool (e.g. sending an email), it validates required parameters via a Zod schema, asks the user for anything missing, then executes

---

## ⚙️ Tech Stack

**Backend**
- Node.js, Express.js
- MongoDB + Mongoose
- LangChain + Google Gemini (`gemini-2.5-flash`)
- JWT authentication, email verification (Nodemailer)
- Deployed on Vercel (serverless)

**Frontend**
- React
- Tailwind CSS
- Tron: Ares–inspired visual theme — black background, molten red accents, futuristic typography
- Context API for Auth / Chat state management

**AI / Agent Layer**
- LangChain's `createAgent` with custom tool-calling
- `sendMail` tool — Zod-validated schema (recipient, subject, body)
- Streaming responses (token-by-token output)

---

## 🔗 Core Architecture

```

User Prompt (React Frontend)
│
▼
Express API (Auth / Chat / Message Controllers)
│
▼
LangChain Agent (Gemini)
├── Analyzes intent & conversation history
├── Validates parameters with Zod schema
└── Decides: Direct Answer OR Tool Execution
│
▼
Tool Layer (e.g. sendEmailTool / Nodemailer)
│
▼
MongoDB (persists User, Chat, and Message records)

```

---

## 🗃️ Data Model

- **User** — auth credentials, verification status
- **Chat** — one document per conversation, references the owning user
- **Message** — one document per message (`role: user | assistant`), references its parent chat

This structure keeps each Chat lightweight (metadata only) while Messages scale independently — a chat with thousands of messages never bloats a single document.

---

## ✅ Features Implemented

- User registration, login, JWT-based auth, email verification
- Create / list chats
- Send message → AI agent generates a reply using full conversation history as context
- Tool-calling — the AI can autonomously send real emails on the user's behalf
- Streaming AI responses
- Persistent chat history across sessions

## 🚧 In Progress / Planned

- Human-in-the-loop confirmation before the agent executes a tool action (safety guardrail)
- Logout, resend-verification endpoints
- Rename / delete chat, delete message
- PDF handling tool
- Cascade delete of messages when a chat is deleted

---

## 🚀 Getting Started

### Prerequisites
- Node.js
- A MongoDB Atlas cluster
- A Google Gemini API key
- A Gmail account with an App Password (for sending emails)

### Backend Setup
```

cd backend
npm install

```

Create a `.env` file in `backend/`:
```

MONGO\_URL=your\_mongodb\_connection\_string
JWT\_SECRET=your\_jwt\_secret
GEMINI\_API\_KEY=your\_gemini\_api\_key
EMAIL\_USER=your\_gmail\_address
EMAIL\_APP\_PASSWORD=your\_gmail\_app\_password

```

Run the server:
```

node server.js

```

### Frontend Setup
```

cd frontend
npm install

```

Create a `.env` file in `frontend/`:
```

VITE\_API\_URL=[http://localhost:3000](http://localhost:3000)

```

Run the dev server:
```

npm run dev

```

---

## 📌 Status

Currently in active development — backend core (auth, chat, messaging, tool-calling agent) is fully functional and tested end-to-end. Frontend is being wired to the backend. Not yet deployed to production.

---

## 💡 Why I Built This

To understand AI engineering at a deeper level than "call an API and display the response" — specifically, how to build systems where an LLM doesn't just answer, but reasons about *when and how to act*, with the safety and architecture considerations that come with letting AI take real-world actions.

---

## 👤 Author

**Prashant Tiwari**
GitHub: [github.com/jai345coder](https://github.com/jai345coder)
LinkedIn: [linkedin.com/in/prashant-tiwari-74284b27b](https://linkedin.com/in/prashant-tiwari-74284b27b)
