# miniVoxSetu

A voice AI assistant for banking questions. You talk into your microphone, it answers out loud — in about a third of a second.

Built by [Harshawardhan Shrivastava](https://github.com/HarrisWarner04).
[Demo video](https://drive.google.com/file/d/1G6ySJ6a4qunmbi7LL7BRzVw6A-p7Ve9t/view?usp=sharing)

## How it works

Your browser sends microphone audio to a Python server. The server:

1. Turns speech into text (Deepgram)
2. Looks up relevant info in a knowledge base (Qdrant + keyword search)
3. Asks an LLM for an answer (Groq, LLaMA 3.3 70B)
4. Turns the answer back into speech and streams it to you

It also listens to *how* you sound — pitch, volume, emotion — and lets you interrupt it mid-sentence, like a real phone call.

## What you need

- Python 3.10+
- Node.js 18+
- Free API keys from [Groq](https://console.groq.com), [Deepgram](https://console.deepgram.com), and [Google AI Studio](https://aistudio.google.com/app/apikey)

## Setup

**1. Get the code**

```bash
git clone https://github.com/HarrisWarner04/ClaudeBuildDay.git
cd ClaudeBuildDay/backend
```

**2. Install the backend**

```bash
python -m venv .venv
.venv\Scripts\activate       # Windows
source .venv/bin/activate    # Mac/Linux

pip install -r requirements.txt
```

**3. Add your API keys**

Create a file called `.env` inside `backend/`:

```env
GROQ_API_KEY=your_key_here
DEEPGRAM_API_KEY=your_key_here
GEMINI_API_KEY=your_key_here
VECTOR_DB_MODE=memory
```

**4. Start the server**

```bash
uvicorn main:app --reload --port 8000
```

**5. Start the frontend** (new terminal)

```bash
cd frontend
npm install
npm run dev
```

Open http://localhost:5173 and click to start talking.

## What's in here

```
backend/
  main.py        FastAPI server, WebSocket, ties everything together
  stt.py         speech → text
  tts.py         text → speech
  rag.py         knowledge base search
  ingest.py      load PDFs/text files into the knowledge base
  acoustic.py    pitch, volume, and emotion from the raw audio
  semantic.py    intent and sentiment analysis (runs in the background)
  pii.py         strips personal info before anything reaches the LLM
  eval_harness.py  benchmarks — run with `python eval_harness.py`
frontend/
  src/App.jsx              the dashboard
  public/pcm-processor.js  captures mic audio in 100ms chunks
```

## Notes

- `VECTOR_DB_MODE=memory` keeps everything in RAM, no setup needed. Set it to `qdrant` and add `QDRANT_URL=http://localhost:6333` if you want a real vector database.
- Never commit your `.env` — it's in `.gitignore` for a reason.
