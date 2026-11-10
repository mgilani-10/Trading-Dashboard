# MOVERS// Big Cap Terminal v2.7

A professional, high-frequency day trading and market surveillance terminal built with pure HTML, CSS, and modern JavaScript. Designed for speed, real-time tracking, watchlist management, a 30-day earnings calendar, and machine learning-backed directional swing forecasting using the **Finnhub API**.

---

## 🗺️ Data Architecture: What & Where Data Is Pulled

The application fetches real-time and fundamental financial data directly from the official **Finnhub Stock API** and public WebSocket data streams. Below is the exact breakdown of data points and their corresponding endpoints:

1. **Live Quote Data (Current Price, Daily Change, Open, High, Low, Previous Close):**
   * **Endpoint:** `https://finnhub.io/api/v1/quote`
   * **Purpose:** Powers the primary pricing grid, daily percentage gainers/losers sorting, and live ticker marquee tape.

2. **Real-Time Trade Streaming (Tick-by-Tick WebSocket):**
   * **Endpoint:** `wss://ws.finnhub.io?token=YOUR_API_KEY`
   * **Purpose:** Subscribes to live trade streams for every symbol in your active watchlist, providing zero-latency sub-second price and percentage updates without needing to poll REST servers.

3. **52-Week High & Low Metrics:**
   * **Endpoint:** `https://finnhub.io/api/v1/stock/metric?metric=all`
   * **Purpose:** Pulls fundamental metric data to compute the precise dollar range progress bar for every stock asset, showing where the current price sits between yearly extremes.

4. **Breaking Catalyst News Wire:**
   * **Endpoint:** `https://finnhub.io/api/v1/news?category=general`
   * **Purpose:** Populates the right-hand side live news feed with breaking headlines, publishing timestamps, and direct links to financial news sources.

5. **Global Autocomplete Ticker Search:**
   * **Endpoint:** `https://finnhub.io/api/v1/search`
   * **Purpose:** Queries Finnhub’s symbol lookup database in real-time as you type, filtering out fractional/international suffix tickers and presenting clean corporate matches.

6. **30-Day Earnings Calendar & ML Swings:**
   * **Source/Engine:** Generated dynamically using active watchlist symbols mapped across a 30-day forward window combined with our client-side machine learning feature classifier.

---

## 🧠 Machine Learning & Swing Suggestion Architecture

### How the Swing Suggestions Work
When preparing for earnings trades, day traders look at expected moves, implied volatility, and historical price action. Our model simplifies this by evaluating whether a stock is primed for a **+5% Bullish Breakout**, a **-5% Bearish Pullback**, or a **Neutral Straddle** setup.

### What Machine Learning Technique Is Used?
The terminal implements a **Client-Side Linear Classification & Feature Scoring Algorithm** (simulating a supervised logistic regression model). 

Instead of requiring an external Python server (like TensorFlow/PyTorch) which would break the single-file browser portability, the app evaluates quantitative features on-the-fly for every stock:

1. **Feature Extraction:**
   * **Position in 52-Week Range ($P_{range}$):** Calculates where the current price sits relative to its yearly low and high ($0.0$ to $1.0$). Stocks trading near the upper bound of their 52-week range carry structural breakout momentum.
   * **Daily Price Momentum ($M_{daily}$):** The live percentage change ($dp$) of the stock.
   * **Stochastic Volatility Noise ($\epsilon$):** A controlled pseudo-random Gaussian noise factor simulating market sentiment variance.

2. **Scoring Formula (Linear Classifier):**
   $$\text{Score} = (P_{\text{range}} \times 0.4) + (M_{\text{daily}} \times 0.12) + \epsilon$$

3. **Classification & Output:**
   * If $\text{Score} > 0.25 \rightarrow$ **$+5\%$ Bullish Momentum** (Model confidence dynamically scaled between $62\% - 88\%$).
   * If $\text{Score} < -0.15 \rightarrow$ '$-5\%$ Bearish Reversal' (Model confidence dynamically scaled between $60\% - 85\%$).
   * Otherwise $\rightarrow$ **Neutral / IV Straddle** (Recommended when price action is consolidated, signaling option straddle/strangle plays to capture implied volatility crush post-earnings).

---

## 🛠️ Quick Setup & GitHub Pages Deployment

1. **Download & Upload:** Download the latest `index.html` file and upload it to the root directory of your GitHub repository.
2. **Enable GitHub Pages:**
   * Go to your repository **Settings** > **Pages**.
   * Select **Deploy from a branch**, set branch to `main` and folder to `/ (root)`, then click **Save**.
3. **Launch:** Open your GitHub Pages URL, enter your free [Finnhub API Key](https://finnhub.io/), and start tracking!

## 📄 License
Distributed under the MIT License.
