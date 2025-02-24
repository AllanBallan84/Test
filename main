from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
import yfinance as yf
import smtplib
import ssl
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import pandas as pd
import numpy as np

app = FastAPI()

# Modell för inkommande data
class PortfolioData(BaseModel):
    stocks: list[str]
    income: float
    expenses: float
    loans: float
    savings: float

# Funktion för att hämta aktiedata
def get_stock_data(stock):
    try:
        data = yf.Ticker(stock).history(period="1y")
        return data['Close'].tolist()
    except:
        return None

# Funktion för att ge finansiella rekommendationer
def generate_recommendations(portfolio: PortfolioData):
    recommendations = []
    
    for stock in portfolio.stocks:
        prices = get_stock_data(stock)
        if prices and len(prices) > 10:
            trend = np.polyfit(range(len(prices)), prices, 1)[0]
            if trend > 0:
                recommendations.append(f"Behåll eller öka innehav i {stock}, positiv trend identifierad.")
            else:
                recommendations.append(f"Överväg att minska innehavet i {stock}, negativ trend identifierad.")
    
    savings_rate = (portfolio.income - portfolio.expenses) / portfolio.income
    if savings_rate < 0.2:
        recommendations.append("Öka ditt sparande, din sparkvot är låg.")
    if portfolio.loans > portfolio.income * 3:
        recommendations.append("Överväg att amortera på lån, hög skuldsättning upptäckt.")
    
    return recommendations

@app.post("/analyze")
def analyze_portfolio(portfolio: PortfolioData):
    recommendations = generate_recommendations(portfolio)
    return {"recommendations": recommendations}

# Funktion för att skicka e-postnotifieringar
def send_email(to_email, subject, body):
    sender_email = "your-email@gmail.com"
    password = "your-email-password"
    
    msg = MIMEMultipart()
    msg['From'] = sender_email
    msg['To'] = to_email
    msg['Subject'] = subject
    msg.attach(MIMEText(body, 'plain'))
    
    try:
        context = ssl.create_default_context()
        with smtplib.SMTP_SSL("smtp.gmail.com", 465, context=context) as server:
            server.login(sender_email, password)
            server.sendmail(sender_email, to_email, msg.as_string())
    except Exception as e:
        print("Error sending email:", e)
