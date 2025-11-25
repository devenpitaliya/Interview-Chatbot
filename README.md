# Interview Chatbot

An AI-powered interview chatbot that conducts natural, role-specific job interviews using LLM technology.

## Features

- 🎯 **Role-Specific Interviews**: Tailored questions based on the job role
- 🤖 **Natural Conversation**: Human-like interview experience powered by Groq LLM
- 📊 **Configurable Question Count**: Easily adjust the number of interview questions
- 💾 **Conversation History**: SQLite database stores all interview sessions
- 🎨 **Clean UI**: User-friendly Streamlit interface
- 🔧 **Industry-Standard Code**: Built with FastAPI, LangChain, and LangGraph

## Architecture

### Backend
- **FastAPI**: REST API for session and chat management
- **LangChain + LangGraph**: Orchestrates conversation flow
- **Groq API**: LLM provider for natural responses
- **SQLite**: Lightweight database for persistence

### Frontend
- **Streamlit**: Interactive web interface with two pages:
  1. Input form (username, role, description)
  2. Chat interface for the interview

## Prerequisites

- Python 3.9+
- Groq API key ([Get one here](https://console.groq.com))

## Installation

1. **Clone or navigate to the project directory**
   ```bash
   cd "Interview app"
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   venv\Scripts\activate  # On Windows
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables**
   - Copy `.env.example` to `.env`
   - Add your Groq API key:
     ```
     GROQ_API_KEY=your_actual_api_key_here
     ```

## Usage

### 1. Start the Backend (FastAPI)

```bash
cd backend
python main.py
```

The API will be available at `http://localhost:8000`

### 2. Start the Frontend (Streamlit)

In a new terminal:

```bash
cd frontend
streamlit run streamlit_app.py
```

The UI will open in your browser at `http://localhost:8501`

### 3. Conduct an Interview

1. Fill in the input form:
   - Your name
   - Interview role (e.g., "Senior Software Engineer")
   - Brief description (optional)

2. Click "Start Interview"

3. Chat naturally with the AI interviewer

4. The interview will automatically end after the configured number of questions

## Configuration

Edit `.env` to customize:

- `MAX_INTERVIEW_QUESTIONS`: Number of questions per interview (default: 10)
- `GROQ_MODEL`: LLM model to use (default: openai/gpt-4o-mini)
- `MODEL_TEMPERATURE`: Response creativity (0.0-1.0, default: 0.7)

## Project Structure

```
Interview app/
├── backend/
│   ├── main.py                 # FastAPI application
│   ├── config.py               # Configuration management
│   ├── models/
│   │   └── schemas.py          # Pydantic models
│   ├── database/
│   │   ├── db.py               # Database setup
│   │   └── crud.py             # Database operations
│   ├── ai_agents/
│   │   ├── graph.py            # LangGraph workflow
│   │   ├── nodes.py            # Graph nodes
│   │   └── state.py            # State definitions
│   └── tools/
│       └── interview_tools.py  # LLM tools
├── frontend/
│   └── streamlit_app.py        # Streamlit UI
├── requirements.txt
├── .env.example
└── README.md
```

## How It Works

1. **Session Creation**: User submits their details, creating a new interview session in SQLite
2. **LangGraph Workflow**: Each user message triggers the interview graph:
   - Interviewer node generates responses using Groq LLM
   - Tools track question count and conversation history
   - Natural conversation flow without hardcoded responses
3. **Graceful Handling**: The LLM handles edge cases:
   - User confusion → rephrases questions
   - Off-topic → politely redirects
   - Early exit → gracefully closes interview
4. **Auto-End**: Interview concludes after reaching the question limit

## API Endpoints

- `GET /` - Health check
- `POST /api/session/create` - Create new interview session
- `GET /api/session/{session_id}` - Get session details
- `POST /api/chat` - Send message and get response

## Future Enhancements

- Interview transcript storage and analysis
- Automated evaluation and scoring
- Multi-language support
- Voice interview capability

## Troubleshooting

**Backend won't start:**
- Ensure `GROQ_API_KEY` is set in `.env`
- Check if port 8000 is available

**Frontend can't connect:**
- Verify backend is running on `http://localhost:8000`
- Check CORS settings in `backend/main.py`

**Database errors:**
- Delete `interview_app.db` and restart the backend

## License

MIT License - feel free to use and modify!

## Support

For issues or questions, please check the code comments or create an issue in the repository.
