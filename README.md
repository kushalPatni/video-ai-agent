# 🎬 AI Video & Meeting Assistant

> **Transcribe · Translate · Summarise · Chat** — Turn any YouTube video or audio file into structured meeting intelligence, powered by local Whisper, Mistral AI, and a RAG pipeline you can actually talk to.

## ✨ What It Does

Paste a YouTube URL (or drop in an audio/video file) and the assistant runs a full pipeline in one click:

| Stage | What happens |
|---|---|
| 🔊 **Audio Extraction** | `yt-dlp` downloads and extracts audio; local files are processed directly |
| 📝 **Transcription** | Local OpenAI Whisper converts speech to text — no API key, no cost |
| 🌐 **Translation** | Auto-detects Hindi/Hinglish and translates to English with `deep-translator` |
| 🏷️ **Title Generation** | Mistral generates a descriptive session title from the transcript |
| 📋 **Summarisation** | LangChain LCEL + Mistral produces a concise meeting summary |
| 🔍 **Extraction** | Pulls out Action Items, Key Decisions, and Open Questions |
| 🧠 **RAG Pipeline** | ChromaDB + HuggingFace embeddings index the transcript for semantic chat |
| 💬 **Chat Interface** | Ask anything about your meeting in natural language |
| 📄 **Export** | Download results as PDF or TXT |

---

## 🏗️ Project Structure

```
ai-meeting-assistant/
│
├── app.py                    # Streamlit UI & pipeline orchestration
│
├── core/
│   ├── transcriber.py        # Whisper transcription logic
│   ├── summarizer.py         # Mistral summarisation + title generation
│   ├── extractor.py          # Action items / decisions / questions extraction
│   └── rag_engine.py         # ChromaDB vector store + RAG chain
│
├── utils/
│   └── audio_processor.py    # yt-dlp download, file chunking, audio prep
│
├── requirements.txt
└── README.md
```

---

## ⚙️ Setup & Installation

### 1. Prerequisites

Install `ffmpeg` — required by Whisper and pydub:

```bash
# Ubuntu / Debian
sudo apt install ffmpeg

# macOS
brew install ffmpeg

# Windows (via Chocolatey)
choco install ffmpeg
```

### 2. Clone the repository

```bash
git clone https://github.com/your-username/ai-meeting-assistant.git
cd ai-meeting-assistant
```

### 3. Create a virtual environment

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
```

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

> ⚠️ **First run downloads Whisper model weights** (~140 MB for `base`, ~1.5 GB for `large`). This is a one-time download.

### 5. Configure environment variables

```bash
cp .env.example .env
```

Open `.env` and add your Mistral API key (free tier works):

```env
MISTRAL_API_KEY=your_mistral_api_key_here
```

Get a free key at → [console.mistral.ai](https://console.mistral.ai)

### 6. Run the app

```bash
streamlit run app.py
```

The app opens at `http://localhost:8501`.

---

## 🚀 Usage

1. **Paste** a YouTube URL or a local file path (`.mp4`, `.mp3`, `.wav`, `.m4a`) in the sidebar
2. **Select** the language — `english` or `hinglish` (auto-translates Hindi to English)
3. **Click** ⚡ Analyse and watch the pipeline status update live
4. **Explore** the summary, action items, decisions, and open questions
5. **Chat** with your transcript using the RAG-powered chat panel
6. **Export** the results as PDF or TXT

---

## 🧠 Tech Stack

| Component | Technology |
|---|---|
| **UI** | Streamlit |
| **Audio Extraction** | yt-dlp, pydub, ffmpeg |
| **Transcription** | OpenAI Whisper (local) |
| **Translation** | deep-translator, langdetect |
| **LLM** | Mistral AI (Free Tier) via `langchain-mistralai` |
| **LLM Orchestration** | LangChain LCEL |
| **Embeddings** | HuggingFace `sentence-transformers` (local) |
| **Vector Store** | ChromaDB |
| **PDF Export** | fpdf2 / ReportLab |

---

## 🌐 Supported Languages

| Language | Support |
|---|---|
| English | ✅ Full transcription + analysis |
| Hindi | ✅ Transcription + auto-translation to English |
| Hinglish | ✅ Mixed Hindi-English — detected and handled |

---

## 💡 Tips & Notes

- **Whisper model size**: The default is `base` (fast, good enough for meetings). Change to `medium` or `large` in `core/transcriber.py` for better accuracy at the cost of speed.
- **Long videos**: Very long recordings are chunked automatically by `utils/audio_processor.py` before transcription.
- **Mistral Free Tier**: The free tier has rate limits. For high-volume use, consider upgrading or adding retry logic via `tenacity` (already in `requirements.txt`).
- **ChromaDB persistence**: By default the vector store is in-memory per session. To persist across sessions, configure a `persist_directory` in `core/rag_engine.py`.
- **GPU acceleration**: If you have a CUDA GPU, Whisper will use it automatically for much faster transcription.

---

## 🛠️ Environment Variables

| Variable | Required | Description |
|---|---|---|
| `MISTRAL_API_KEY` | ✅ Yes | Mistral AI API key (free tier works) |

---

## 📋 Requirements

- Python **3.10+**
- `ffmpeg` binary on system PATH
- ~2 GB disk space for Whisper model weights (first run)
- Internet connection (for YouTube downloads and Mistral API calls)

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

1. Fork the repo
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 Acknowledgements

- [OpenAI Whisper](https://github.com/openai/whisper) — Local speech recognition
- [LangChain](https://github.com/langchain-ai/langchain) — LLM orchestration
- [Mistral AI](https://mistral.ai) — LLM backbone
- [ChromaDB](https://www.trychroma.com) — Vector store
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) — YouTube audio extraction
- [Streamlit](https://streamlit.io) — UI framework

---

<div align="center">
  <sub>Built with ❤️ — paste a link, get intelligence.</sub>
</div>
