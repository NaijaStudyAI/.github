# Nigerian-Accented AI Study Assistant on WhatsApp

A **Generative AI Study Companion** integrated with **WhatsApp** that helps users interact with their study materials via text or voice. The unique value proposition is **Voice Mode** with Nigerian-accented English responses using YarnGPT.

## Features

- **Document Ingestion**: Upload PDFs/DOCX → Extract text → Store in Vector DB
- **RAG-powered Q&A**: Answers grounded in uploaded study materials
- **Voice Input**: Send WhatsApp Voice Notes (transcribed via Groq Whisper)
- **Nigerian TTS Output**: AI responds with audio in Nigerian accent (YarnGPT)
- **Multi-language Support**: English, Pidgin, Yoruba, Hausa, Igbo

## Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        WhatsApp Cloud API                                │
└──────────────────────────────┬──────────────────────────────────────────┘
                               │
                               ▼
┌─────────────────────────────────────────────────────────────────────────┐
│  Lambda A (Receiver)          │  Lambda B (Processor)                    │
│  ─────────────────            │  ─────────────────────                   │
│  • FastAPI + Mangum           │  • STT (Groq Whisper)                    │
│  • Verify webhook             │  • RAG (Pinecone + Google Embeddings)    │
│  • Push to SQS                │  • LLM (Groq Llama 3.1)                  │
│  • Return 200 OK              │  • TTS (YarnGPT Nigerian)                │
└──────────────┬────────────────┴──────────────────────────────────────────┘
               │                                    │
               ▼                                    │
┌──────────────────────┐                            │
│     Amazon SQS       │◄───────────────────────────┘
│    (Job Queue)       │
└──────────────────────┘
               │
               ▼
┌──────────────────────────────────────────────────────────────────────────┐
│                           Data Layer                                      │
├──────────────────┬──────────────────────┬────────────────────────────────┤
│   DynamoDB       │   Pinecone           │   S3                           │
│   (User State)   │   (Vector DB)        │   (Documents/Audio)            │
└──────────────────┴──────────────────────┴────────────────────────────────┘
```

## Tech Stack

| Component | Technology |
|-----------|------------|
| Language | Python 3.10+ |
| Framework | FastAPI |
| Cloud | AWS (Lambda, API Gateway, SQS, DynamoDB, S3) |
| LLM | Groq API (Llama 3.1 8B / 3.3 70B) |
| STT | Groq API (Whisper Large v3) |
| TTS | YarnGPT API (Nigerian Accents) |
| Embeddings | Google (gemini-embedding-001) |
| Vector DB | Pinecone (Serverless) |

## Voice Mode

Users can toggle voice responses:

- **"Voice On"** → AI responds with Nigerian-accented audio + text summary
- **"Voice Off"** → AI responds with text only

## License

MIT
