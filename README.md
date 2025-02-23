# Finance AI Backend
En enkel FastAPI-backend för att analysera aktier med Yahoo Finance.

## Installation
```
pip install -r requirements.txt
```

## Kör servern
```
uvicorn src.main:app --host 0.0.0.0 --port 8000 --reload
```

## API-endpoints
- `/` - Startsidan
- `/analyze?ticker=AAPL` - Analyserar en aktie (byt AAPL mot annat bolag)
