# TRAVELPILOT ✈️🤖
### Intelligent Trip Planning & Disruption Management Agent

TravelPilot is an end-to-end, production-style AI travel planning and real-time disruption management system. It leverages **LangGraph**, **LangChain**, and **Google Gemini** for intelligent multi-step itinerary creation, schedule validation, rain/cancellation conflict resolution, budget optimization, and interactive natural language modification.

---

## 🌟 Key Features

1. **Autonomous LangGraph Planning Pipeline**:
   - 13-node state execution graph (Requirements -> Tavily Search -> Weather -> Geocoding -> Travel Times -> Budget -> Draft Itinerary -> Opening Hours Validation -> Conflict Detector -> Optimization -> Final Itinerary).
2. **Real-Time Disruption Engine**:
   - Scans rain forecasts and activity opening hours. Automatically replaces outdoor walking tours with indoor alternatives when rain is detected.
3. **Natural Language Travel Assistant**:
   - Interactive chat assistant aware of itinerary state. Ask questions like:
     - *"What should I do tomorrow morning?"*
     - *"Reduce my total budget by 20%"*
     - *"What happens if it rains tomorrow?"*
     - *"My activity was cancelled. What should I do instead?"*
4. **Interactive Dashboard & Recharts**:
   - Category spending pie & bar charts (Accommodation, Dining, Transport, Activities, Miscellaneous).
   - Spatial route explorer & geocoded pin location details.
   - Expandable backup option preview cards for all activities.

---

## 🔑 Where to Paste Your API Keys

Copy `.env.example` to `.env` in the root directory:

```bash
cp .env.example .env
```

Open `.env` and fill in your keys:

```env
# Required for AI Itinerary Generation & Natural Language Chat
# Get key from Google AI Studio: https://aistudio.google.com/
GEMINI_API_KEY=your_gemini_api_key_here

# Required for Live Web Search (Attractions, Dining, Events)
# Get key from Tavily: https://tavily.com/
TAVILY_API_KEY=your_tavily_api_key_here

# Optional: Google Maps & Places API key for real geocoding
GOOGLE_MAPS_API_KEY=

# Optional: Weather API key (OpenWeatherMap)
WEATHER_API_KEY=

# Database & Auth Defaults
DATABASE_URL=sqlite:///./travelpilot.db
JWT_SECRET=super-secret-travelpilot-jwt-key
GEMINI_MODEL=gemini-2.5-flash
```

> [!NOTE]
> If API keys are missing or pending during initial startup, TravelPilot uses smart fallback rule-based generators for weather/maps to ensure the application remains fully operable locally without crashing.

---

## 🚀 How to Run Locally

### 1. Start Backend (FastAPI)

```bash
# Create Python virtual environment
python -m venv venv
# Windows:
venv\Scripts\activate
# Linux/macOS:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run FastAPI backend server (Port 8000)
uvicorn backend.main:app --host 0.0.0.0 --port 8000 --reload
```

The backend API documentation will be available at [http://localhost:8000/docs](http://localhost:8000/docs).

### 2. Start Frontend (React + Vite + Tailwind)

In a separate terminal:

```bash
cd frontend

# Install Node dependencies
npm install

# Start Vite dev server (Port 3000)
npm run dev
```

Open your browser at [http://localhost:3000](http://localhost:3000).

---

## 🐳 Running with Docker & Docker-Compose

To launch the full production stack using Docker:

```bash
docker-compose up --build
```

- Frontend: [http://localhost:3000](http://localhost:3000)
- Backend API: [http://localhost:8000](http://localhost:8000)

---

## 🧪 Running Automated Tests

Run backend unit tests for tools, weather calculation, and schedule conflict detection:

```bash
pytest tests/
```

---

## 📁 Project Architecture

```text
travelpilot/
├── backend/
│   ├── agents/          # LangGraph high-level agent & Chat agent
│   ├── api/             # FastAPI endpoints (auth, trips, planning, budget, disruptions, chat)
│   ├── database/        # SQLAlchemy session & init
│   ├── graph/           # 13-node LangGraph execution state graph
│   ├── models/          # Database models (User, Trip, Itinerary, Activity, Disruption)
│   ├── schemas/         # Pydantic validation schemas
│   ├── services/        # Gemini LLM service & Auth service
│   ├── tools/           # Tavily search, weather, maps, budget, conflict tools
│   ├── config.py        # Centralized settings & API key validation
│   └── main.py          # Application entrypoint
├── frontend/
│   ├── src/
│   │   ├── components/  # ActivityCard, WeatherWidget, PlanningStepper, DisruptionCard, ChatWidget
│   │   ├── pages/       # Landing, Login, Register, TripCreation, AIPlanning, Dashboard, Itinerary, Map, Budget, DisruptionCenter, Chat
│   │   ├── services/    # Axios API client
│   │   └── App.tsx      # Routing layout
│   └── package.json
├── tests/               # Pytest suite
├── docker-compose.yml
├── .env.example
├── .gitignore
├── README.md
└── requirements.txt
```
