# Setup Guide - AI-Based Illegal Fishing & Zone Violation Alert System

## Prerequisites

- Python 3.8 or higher
- Node.js 16+ and npm/yarn
- Git

## Backend Setup

1. **Navigate to backend directory:**
```bash
cd backend
```

2. **Create virtual environment (recommended):**
```bash
python -m venv venv

# On Windows:
venv\Scripts\activate

# On Linux/Mac:
source venv/bin/activate
```

3. **Install Python dependencies:**
```bash
pip install -r requirements.txt
```

4. **Train the GRU model (optional but recommended):**
```bash
python train_gru.py
```
This will create `models/gru_model.pt` and `models/scaler.json`. If these files don't exist, the system will use simple physics-based predictions.

5. **Start the FastAPI server:**
```bash
python main.py
```
Or using uvicorn directly:
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

The backend will be available at `http://localhost:8000`

## Frontend Setup

1. **Navigate to project root:**
```bash
cd ..
```

2. **Install Node.js dependencies:**
```bash
npm install
```

3. **Create environment file (optional):**
```bash
# Create .env file with:
VITE_API_URL=http://localhost:8000
```

4. **Start the development server:**
```bash
npm run dev
```

The frontend will be available at `http://localhost:5173`

## API Endpoints

### Backend Endpoints

- `POST /ingest` - Receive GPS data
  ```json
  {
    "vessel_id": "vessel-001",
    "lat": 8.7642,
    "lon": 78.1348,
    "speed": 12.5,
    "heading": 45.0
  }
  ```

- `POST /predict` - Predict next position
  ```json
  {
    "vessel_id": "vessel-001",
    "current_lat": 8.7642,
    "current_lon": 78.1348,
    "speed": 12.5,
    "heading": 45.0
  }
  ```

- `GET /alerts` - Get active alerts
  Query params: `vessel_id` (optional), `limit` (default: 50)

- `GET /zones` - Get all zones

- `POST /zones` - Add a new zone
  ```json
  {
    "name": "New Restricted Zone",
    "zone_type": "restricted",
    "coordinates": [
      {"lat": 8.73, "lon": 78.18},
      {"lat": 8.75, "lon": 78.20},
      {"lat": 8.77, "lon": 78.18},
      {"lat": 8.75, "lon": 78.16}
    ]
  }
  ```

- `GET /vessels/{vessel_id}/history` - Get vessel history
  Query params: `limit` (default: 100)

- `GET /health` - Health check

## Database

The backend uses SQLite database (`fishing_zone.db`) which is automatically created on first run. It stores:
- Vessel GPS data
- Alerts
- Zone definitions

## Testing

### Backend Testing

Run pytest tests (if available):
```bash
cd backend
pytest
```

### Manual Testing

1. Start both backend and frontend
2. Open the frontend in browser
3. Navigate to Dashboard or Map page
4. Click "Start Tracking" to begin GPS simulation
5. Watch for alerts when entering restricted zones

## Troubleshooting

### Backend Issues

- **Port already in use**: Change port in `main.py` or use `--port` flag with uvicorn
- **Model not found**: Run `python train_gru.py` to generate the model
- **Database errors**: Delete `fishing_zone.db` and restart (will recreate)

### Frontend Issues

- **API connection errors**: Ensure backend is running on port 8000
- **CORS errors**: Check backend CORS settings in `main.py`
- **Build errors**: Run `npm install` again

## Project Structure

```
.
├── backend/
│   ├── main.py                 # FastAPI application
│   ├── train_gru.py            # Model training script
│   ├── requirements.txt        # Python dependencies
│   ├── modules/
│   │   ├── database.py         # SQLite database manager
│   │   ├── data_simulation.py  # Data simulation & Kalman filter
│   │   ├── gru_model.py        # GRU prediction model
│   │   ├── geofencing.py       # Zone detection
│   │   └── alert_generator.py  # Alert generation
│   ├── models/                 # Trained models (generated)
│   ├── data/                   # Zone data (generated)
│   └── fishing_zone.db        # SQLite database (generated)
├── src/
│   ├── services/
│   │   └── api.ts              # API service layer
│   ├── pages/
│   │   ├── Dashboard.tsx       # Dashboard page
│   │   └── MapPage.tsx         # Map page
│   └── ...
└── SETUP.md                    # This file
```

## Features

✅ GPS data ingestion and storage
✅ GRU-based position prediction
✅ Geofencing with zone detection
✅ Real-time alerts (voice + visual)
✅ SQLite database for persistence
✅ RESTful API endpoints
✅ React frontend with live updates
✅ Multi-language support (frontend)

## Next Steps

1. Train the GRU model with real vessel data for better predictions
2. Add more zones via the `/zones` endpoint
3. Integrate with real GPS hardware
4. Add authentication/authorization
5. Deploy to production

