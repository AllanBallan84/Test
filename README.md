# Finance AI Backend
En enkel FastAPI-backend för att analysera aktier med Yahoo Finance.

## Installation
```
pip install -r requirements.txt
```

## Kör servern
```
uvicorn main:app --host 0.0.0.0 --port 8000
```

## API-endpoints
- `/` - Startsidan
- `/analyze?ticker=AAPL` - Analyserar en aktie (byt AAPL mot annat bolag)
