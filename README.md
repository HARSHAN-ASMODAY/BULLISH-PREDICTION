
# 📈 Bullish Tomorrow — NSE Stock Prediction Bot

A **rule-based Python bot** that predicts if selected NSE (India) stocks will **rise or fall tomorrow** based on technical analysis indicators.  
The bot runs daily after market close, saves predictions to a **PostgreSQL database**, and emails the results.  
A **React + JavaScript frontend** will later fetch and display predictions from the database.

---

## 🚀 Features
- ✅ Fetches daily NSE stock data from Yahoo Finance (`yfinance`)
- ✅ Calculates SMA20, RSI14, MACD, AvgVol20, and daily % change
- ✅ Applies scoring rules to decide if a stock will **rise** or **fall** tomorrow
- ✅ Saves results in **PostgreSQL**
- ✅ Sends formatted email with predictions
- ✅ Ready for integration with a React frontend

---

## 📊 How It Works

**Daily Workflow:**
1. **Trigger** — Script runs at ~4:00 PM IST (after NSE close).
2. **Data Fetch** — Gets OHLCV data for chosen tickers.
3. **Indicators** — Computes:
   - Simple Moving Average (20 days)
   - Relative Strength Index (14 days)
   - MACD histogram
   - Average Volume (20 days)
   - Daily % change
4. **Rules** — Scores each stock:
   | Rule | Condition | Points |
   |------|-----------|--------|
   | Trend | Close > SMA20 | +1 |
   | Daily gain | Close > Prev Close | +1 |
   | Volume spike | Volume > AvgVol20 | +1 |
   | RSI healthy | RSI < 70 | +1 |
   | MACD positive | MACD hist > 0 | +1 |
   | Market bias | NIFTY close > prev close | +1 |
5. **Prediction** — If score ≥ 4 → “Will rise tomorrow”, else “Will fall tomorrow”.
6. **Database** — Saves predictions in PostgreSQL.
7. **Email** — Sends the day’s predictions to your inbox.

---

## 🛠️ Tech Stack
**Backend:**
- Python 3.9+
- PostgreSQL

**Libraries:**
- `pandas`, `numpy` — Data handling
- `pandas_ta` — Technical indicators
- `yfinance` — Market data
- `SQLAlchemy`, `psycopg2` — PostgreSQL ORM/connector
- `smtplib`, `email` — Email sending

**Frontend (Future):**
- React + JavaScript

---

## 📂 Project Structure
```

bullish-tomorrow/
│
├── bot.py                  # Main script
├── config.py               # Configuration (API keys, DB, email settings)
├── requirements.txt        # Python dependencies
├── db\_setup.sql            # SQL to create required tables
├── README.md               # Project documentation
└── utils/
├── indicators.py       # Functions for technical indicators
├── rules.py            # Rule-checking logic
├── email\_sender.py     # Email sending helper
├── db\_handler.py       # Database save/load functions

````

---

## 🗄️ Database Structure (PostgreSQL)
```sql
CREATE TABLE predictions (
    id SERIAL PRIMARY KEY,
    date DATE NOT NULL,
    stock_name VARCHAR(20) NOT NULL,
    prediction VARCHAR(50) NOT NULL,
    score INTEGER NOT NULL,
    sma20 NUMERIC,
    rsi14 NUMERIC,
    macd_hist NUMERIC,
    avg_vol20 NUMERIC,
    daily_change NUMERIC
);
````

---

## ⚙️ Installation & Setup

### 1️⃣ Clone the repo

```bash
git clone https://github.com/yourusername/bullish-tomorrow.git
cd bullish-tomorrow
```

### 2️⃣ Create and activate a virtual environment

```bash
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows
```

### 3️⃣ Install dependencies

```bash
pip install -r requirements.txt
```

### 4️⃣ Configure database & email

Edit `config.py` with your PostgreSQL and email credentials:

```python
DB_URI = "postgresql+psycopg2://user:password@localhost:5432/bullishdb"
EMAIL_SENDER = "your@email.com"
EMAIL_PASSWORD = "yourpassword"
EMAIL_RECEIVER = "receiver@email.com"
```

### 5️⃣ Create database table

```bash
psql -U youruser -d bullishdb -f db_setup.sql
```

### 6️⃣ Run the bot

```bash
python bot.py
```

---

## ⏱️ Scheduling (Daily Run)

### On Linux/Mac (cron job):

```bash
0 16 * * 1-5 /path/to/venv/bin/python /path/to/bot.py
```

### On Windows (Task Scheduler):

* Create a daily task to run `python bot.py` at 4:00 PM IST.

---

## 📧 Example Email Output

```
Subject: Stock Predictions — 2025-08-08

RELIANCE.NS — Will rise tomorrow
TCS.NS — Will fall tomorrow
INFY.NS — Will rise tomorrow
```

---

## 📈 Backtesting

* Modify `bot.py` to fetch historical data.
* Apply the same rules to 1–2 years of past data.
* Calculate accuracy and adjust thresholds.

---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss.

---

## 👨‍💻 Author

**Harshan** — Building intelligent trading systems for Indian stock markets.

```

---

If you want me to help generate **requirements.txt** or sample code files next, just ask!
```
