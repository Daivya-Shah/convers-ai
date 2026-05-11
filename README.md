# Convers-AI

Convers-AI is an AI-powered learning platform that lets you chat with your documents and generate flashcard decks from your own content. Upload a PDF, paste some notes, or drop in a link and the app will use that as context for everything the AI says.

![Architecture](public/Architecture.png)

---

## Features

- **Contextual AI Chat** — ask questions and get answers grounded in your uploaded documents, not just general knowledge
- **AI Flashcard Generation** — turn any text or PDF into a ready-to-study flashcard deck in seconds
- **PDF Upload in Chat** — attach a document mid-conversation to update the AI's knowledge on the fly
- **Persistent Conversations** — all chat history is saved so you can pick up where you left off
- **Subscription Plans** — Free, Premium, and Team tiers with Stripe payments

---

## Tech Stack

| Area | Technology |
|---|---|
| Framework | Next.js 14 (App Router), TypeScript |
| Styling | Tailwind CSS |
| Auth | Clerk |
| AI | OpenAI GPT-4o-mini via LangChain |
| Vector Store | Pinecone |
| Database | MongoDB |
| Payments | Stripe |
| Analytics | Vercel Analytics |

---

## How It Works

### Chat (RAG Pipeline)

1. You type a message
2. The app searches Pinecone for document chunks relevant to your question
3. Those chunks are injected into the prompt as context alongside your conversation history
4. GPT-4o-mini streams back a response grounded in your documents
5. Both your message and the AI's response are saved to MongoDB

### Flashcard Generation

1. You provide content (PDF, text, or link) and give the deck a name
2. The content gets chunked and embedded into Pinecone
3. Keywords are extracted from your content, then used to retrieve the most relevant chunks
4. GPT-4o-mini generates 10 flashcards (question + answer) from that context
5. The deck is saved to MongoDB and shows up in your dashboard

---

## Getting Started

### Prerequisites

- Node.js 18+
- Accounts and API keys for: [Clerk](https://clerk.com), [OpenAI](https://platform.openai.com), [Pinecone](https://pinecone.io), [MongoDB Atlas](https://mongodb.com/atlas) (or a local instance)
- Stripe keys (optional, only needed for the subscription page)

### Installation

```bash
git clone https://github.com/your-username/convers-ai.git
cd convers-ai
npm install
```

### Environment Setup

Copy the example env file and fill in your keys:

```bash
cp .env.example .env
```

```env
# Clerk
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
CLERK_SECRET_KEY=your_clerk_secret_key
CLERK_SIGN_IN_URL=/sign-in
CLERK_AFTER_SIGN_IN_URL=/dashboard

# OpenAI
OPENAI_API_KEY=your_openai_api_key

# Pinecone
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_ENVIRONMENT=your_pinecone_environment
PINECONE_INDEX_NAME=your_index_name
INDEX_INIT_TIMEOUT=240000

# MongoDB
MONGODB_URI=your_mongodb_uri

# Stripe (optional)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
STRIPE_SECRET_KEY=your_stripe_secret_key
```

### Running the App

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). You'll be redirected to sign in, and then land on your dashboard.

---

## Project Structure

```
app/
  api/              # All API routes (chat, flashcards, documents, payments)
  components/       # Shared UI components
  dashboard/        # Dashboard pages (chat, flashcards, create)
  lib/              # Database clients, Pinecone client, vector store helpers
  scripts/          # One-off scripts (e.g. pre-embedding docs into Pinecone)
  types/            # Shared TypeScript types
public/             # Static assets
```

---

## Roadmap

- [ ] Per-conversation URL routes
- [ ] Optimized database structure
- [ ] Update chat context automatically after PDF upload
- [ ] Tab for managing uploaded documents and removing specific context
- [ ] Google Analytics
- [ ] Fine-tune model for conversation prediction
- [ ] Fix bug with new chat creation after the 10th conversation
- [ ] Codebase refactor and component cleanup

---

## Contributing

Pull requests are welcome. For larger changes, open an issue first to discuss what you'd like to change.
