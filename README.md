# Stock Portfolio Construction Framework

A systematic, data-driven approach to building diversified investment portfolios using fundamental, technical, and sentiment analysis of S&P 500 stocks.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![Pandas](https://img.shields.io/badge/Pandas-2.0+-green.svg)
![TA-Lib](https://img.shields.io/badge/TA--Lib-0.4.28-orange.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)

<img width="6719" height="6003" alt="visualization" src="https://github.com/user-attachments/assets/d4779d8f-ad36-4bfa-b7d9-b255108292b5" />

## 🎯 TL;DR

A quantitative portfolio construction system that:
- **Analyzes all S&P 500 stocks** across fundamental, technical, and sentiment dimensions.
- **Generates composite ratings** using percentile-based scoring.
- **Constructs optimal portfolios** with sector-balanced allocation strategies.
- **Visualizes portfolio composition** through multi-layered pie charts.
- **Automates the entire workflow** from data collection to portfolio generation.

Perfect for quantitative analysts, portfolio managers, and algorithmic traders seeking systematic investment strategies.

## 💡 Problem/Motivation

Building a well-diversified portfolio requires analyzing hundreds of stocks across multiple dimensions:

### Traditional Challenges
- **Time-intensive research**: Manually analyzing 500+ stocks takes weeks.
- **Subjective bias**: Human judgment can lead to emotional or biased decisions.
- **Inconsistent methodology**: Different analysts use different frameworks.
- **Rebalancing complexity**: Maintaining sector balance requires constant monitoring.
- **Data fragmentation**: Fundamental, technical, and sentiment data live in different systems.

### Solution
This framework provides a **systematic, repeatable process** that:
1. Collects data from multiple sources automatically.
2. Applies consistent scoring methodology across all stocks.
3. Generates unbiased ratings based on quantitative metrics.
4. Constructs portfolios with predefined sector allocations.
5. Visualizes the entire investment strategy in a single diagram.

## 📊 Data Description

### Data Pipeline

```
Wikipedia S&P 500 → Tickers → yfinance API → Market Data
                                    ↓
                    ┌───────────────┼───────────────┐
                    ↓               ↓               ↓
              Fundamental      Technical      Sentimental
                  Data           Data           Data
                    ↓               ↓               ↓
                    └───────────────┼───────────────┘
                                    ↓
                              Rating System
                                    ↓
                          Portfolio Construction
                                    ↓
                            Visualization
```

### Data Sources

| Source | Purpose | Data Points | Update Method |
|--------|---------|-------------|---------------|
| **Wikipedia** | S&P 500 ticker list | 500+ symbols | Web scraping |
| **yfinance** | Financial statements & price data | OHLCV, balance sheet, income statement | API calls |
| **Reddit (PRAW)** | Community sentiment | Post text from r/wallstreetbets | API scraping |
| **TA-Lib** | Technical indicators | 8 calculated indicators | Computed |

### Metrics Calculated

#### Fundamental (10 metrics)
**Valuation** (6 metrics):
- P/E Ratio (PER).
- Price/Earnings to Growth (PEGR).
- Price-to-Book (PBR).
- Dividend Yield (DY).
- EV/EBITDA.
- Price-to-Sales (P/S).

**Profitability** (2 metrics):
- Return on Assets (ROA).
- Net Profit Margin (NPM).

**Financial Health** (2 metrics):
- Interest Coverage Ratio (ICR).
- Free Cash Flow (FCF).

**Growth** (2 metrics):
- Revenue Growth Rate (RGR).
- Earnings Growth Rate (EGR).

#### Technical (8 metrics)
**Trend** (4 indicators):
- Simple Moving Average (SMA-200).
- Exponential Moving Average (EMA-100).
- MACD (12/26/9).
- Average Directional Index (ADX-30).

**Momentum** (2 indicators):
- Relative Strength Index (RSI-60).
- Rate of Change (ROC-90).

**Volume** (2 indicators):
- On-Balance Volume (OBV).
- Money Flow Index (MFI-30).

#### Sentiment (1 metric):
- Reddit Sentiment Score: TextBlob polarity analysis of r/wallstreetbets posts (1-year history).

## 📁 Project Structure

```
stock-portfolio-construction/
│
├── fundamental.ipynb          # Collects & calculates fundamental metrics
├── technical.ipynb            # Collects & calculates technical indicators
├── sentimental.ipynb          # Scrapes & analyzes Reddit sentiment
├── rating.ipynb               # Generates composite ratings via percentile scoring
├── portfolio.ipynb            # Constructs optimized portfolios
├── visualization.ipynb        # Creates portfolio visualization charts
│
├── Data/                      # Generated data files
│   ├── fundamental.csv        # Fundamental metrics for all stocks
│   ├── technical.csv          # Technical indicators for all stocks
│   ├── sentimental.csv        # Sentiment scores for all stocks
│   ├── rating.csv             # Composite ratings (fundamental + technical + sentiment)
│   ├── analysis.csv           # Complete analysis with company metadata
│   ├── portfolio.csv          # Final portfolio composition
│   └── visualization.png      # Portfolio construction diagram
│
├── requirements.txt           # Python dependencies
└── README.md                  # This file
```

### Key Dependencies

```
pandas==2.1.3
numpy==1.26.2
yfinance==0.2.32
TA-Lib==0.4.28
praw==7.7.1
textblob==0.17.1
nltk==3.8.1
matplotlib==3.8.2
emoji==2.8.0
```

## 🔬 Methodology

### Stage 1: Data Collection

#### 1.1 Fundamental Analysis (`fundamental.ipynb`)
```python
For each S&P 500 stock:
    1. Download quarterly financial statements (yfinance).
    2. Extract balance sheet, income statement, cash flow data.
    3. Calculate 10 fundamental metrics.
    4. Handle missing values via sector-median imputation.
    5. Export to fundamental.csv.
```

**Key Features**:
- Uses most recent quarterly data.
- Handles stocks with incomplete data gracefully.
- Sector-aware missing value imputation.
- Removes 8 stocks with excessive missing data.

#### 1.2 Technical Analysis (`technical.ipynb`)
```python
For each S&P 500 stock:
    1. Download 1-year daily OHLCV data.
    2. Calculate 8 technical indicators using TA-Lib.
    3. Extract most recent indicator values.
    4. Export to technical.csv.
```

**Indicator Periods**:
- SMA: 200 days (long-term trend).
- EMA: 100 days (intermediate trend).
- MACD: 12/26/9 (standard settings).
- ADX: 30 days (trend strength).
- RSI: 60 days (momentum).
- ROC: 90 days (rate of change).
- MFI: 30 days (volume-weighted RSI).
- OBV: Cumulative (volume flow).

#### 1.3 Sentiment Analysis (`sentimental.ipynb`)
```python
For each S&P 500 stock:
    1. Scrape 1 year of posts from r/wallstreetbets.
    2. Search using ticker symbol (e.g., "AAPL OR $AAPL").
    3. Clean text: remove URLs, emojis, stop words.
    4. Lemmatize words for consistency.
    5. Calculate TextBlob polarity (-1 to +1).
    6. Average all post sentiments per stock.
    7. Export to sentimental.csv.
```

**Text Preprocessing**:
```
Raw Post → Lowercase → Remove URLs/Emojis → Remove Special Chars →
Tokenize → Remove Stop Words → Lemmatize → TextBlob Polarity
```

### Stage 2: Rating System (`rating.ipynb`)

#### 2.1 Percentile Transformation
All metrics are converted to percentile scores (1-100):

```python
def percentile(series, invert=False):
    ranks = series.rank(pct=True)  # 0.0 to 1.0
    if invert:  # For metrics where lower is better
        ranks = 1 - ranks
    scores = floor(ranks * 100) + 1  # 1 to 100
    return scores
```

**Inverted Metrics** (lower is better):
- PER, PBR, EV/EBITDA, P/S.

**Normal Metrics** (higher is better):
- All others.

#### 2.2 Composite Score Calculation

**Fundamental Score** (50% weight):
```
Fundamental = (
    Valuation(PER, PEGR, PBR, DY, EV/EBITDA, P/S) × 0.40 +
    Profitability(ROA, NPM) × 0.20 +
    Health(ICR, FCF) × 0.20 +
    Growth(RGR, EGR) × 0.20
)
```

**Technical Score** (30% weight):
```
Technical = (
    Trend(SMA, EMA, MACD, ADX) × 0.50 +
    Momentum(RSI, ROC) × 0.30 +
    Volume(OBV, MFI) × 0.20
)
```

**Sentiment Score** (20% weight):
```
Sentiment = Reddit Score (already 1-100)
```

**Final Rating**:
```
Rating = (
    Fundamental × 0.50 +
    Technical × 0.30 +
    Sentiment × 0.20
)
```

All composite scores are re-percentiled to ensure final ratings are 1-100.

### Stage 3: Portfolio Construction (`portfolio.ipynb`)

#### 3.1 Portfolio Parameters
```python
Budget: €5,000

Target Allocation:
    Growth:     50% (€2,500)
    Defensive:  30% (€1,500)
    Cyclical:   20% (€1,000)

Sector Weights within Types:
    Growth:
        - Information Technology: 40%
        - Communication Services: 40%
        - Consumer Discretionary: 20%
    
    Defensive:
        - Health Care: 40%
        - Consumer Staples: 40%
        - Utilities: 20%
    
    Cyclical:
        - Financials: 40%
        - Industrials: 20%
        - Materials: 20%
        - Energy: 20%
```

#### 3.2 Stock Selection Algorithm
```python
For each sector:
    1. Allocate budget based on sector weight.
    2. Select top 20 stocks by Rating.
    3. Purchase 1 share of highest-rated stocks until budget exhausted.
    4. Allow 10% budget overage for rounding.
    5. Stop when 90% of sector budget is allocated.
```

**Constraints**:
- Only 1 share per stock (simplicity).
- Minimum 90% budget utilization per sector.
- Maximum 110% budget utilization per sector.
- Stocks must have complete data (no missing ratings).

#### 3.3 Output
Generates two CSV files:
- `analysis.csv`: All 490+ stocks with ratings, prices, and metadata.
- `portfolio.csv`: Selected stocks with shares, prices, and investment amounts.

### Stage 4: Visualization (`visualization.ipynb`)

Creates a comprehensive 4-panel visualization:

```
┌─────────────────────────────────────┐
│    PORTFOLIO CONSTRUCTION           │
├─────────────────────────────────────┤
│          Analysis (SP500)           │
├──────────────┬──────────────────────┤
│ Types &      │ Variables            │
│ Categories   │ (20+ metrics)        │
│ - Nested pie │ - Single-layer pie   │
│ - Inner:     │ - Color-coded by     │
│   F/T/S      │   type               │
│ - Outer:     │                      │
│   Categories │                      │
├─────────────────────────────────────┤
│        Portfolio (€5000)            │
├──────────────┬──────────────────────┤
│ Types &      │ Stocks               │
│ Sectors      │ (Individual)         │
│ - Nested pie │ - Single-layer pie   │
│ - Inner:     │ - Color-coded by     │
│   G/D/C      │   type               │
│ - Outer:     │ - Shows all holdings │
│   Sectors    │                      │
└──────────────┴──────────────────────┘
```

**Color Scheme**:
- Blue: Growth / Fundamental.
- Green: Defensive / Technical.
- Yellow: Cyclical / Sentiment.

## 📈 Results/Interpretation

### Sample Portfolio Output

**Portfolio Composition** (€5,000 budget):
```
Total Stocks: 35-45 (varies by market prices)
Total Investment: €4,850-5,100 (±5% of budget)

Type Distribution:
    Growth:     ~€2,500 (50%)
    Defensive:  ~€1,500 (30%)
    Cyclical:   ~€1,000 (20%)

Sector Distribution:
    Information Technology: ~€1,000 (20%)
    Health Care:           ~€600 (12%)
    Financials:            ~€400 (8%)
    Communication Services: ~€1,000 (20%)
    Consumer Staples:      ~€600 (12%)
    Industrials:           ~€200 (4%)
    Consumer Discretionary: ~€500 (10%)
    Materials:             ~€200 (4%)
    Energy:                ~€200 (4%)
    Utilities:             ~€300 (6%)
```

### Rating Distribution Insights

Typical rating distribution across S&P 500:
- **Top Quartile (75-100)**: 125 stocks → High-conviction holdings.
- **2nd Quartile (50-75)**: 125 stocks → Moderate holdings.
- **3rd Quartile (25-50)**: 125 stocks → Avoid or short candidates.
- **Bottom Quartile (1-25)**: 125 stocks → Strong avoid.

**Portfolio Selection**: Typically draws from top quartile plus some 2nd quartile stocks for sector balance.

### Interpretation Examples

**High-Rated Stock (Rating: 85)**:
- Strong fundamentals (low P/E, high ROA).
- Positive technical momentum (price above SMA/EMA, RSI 50-70).
- Positive sentiment (community buzz).
- **Action**: Core holding candidate.

**Low-Rated Stock (Rating: 25)**:
- Weak fundamentals (high P/E, negative growth).
- Negative technical signals (price below moving averages).
- Negative sentiment (community concerns).
- **Action**: Avoid or consider for short strategy.

## 💼 Business Impact

### For Retail Investors
- **Democratized quantitative analysis**: Access institutional-grade methodologies.
- **Reduced emotional bias**: Systematic, rules-based selection.
- **Educational tool**: Understand how professionals analyze stocks.
- **Time efficiency**: 30 minutes vs 30 hours for portfolio construction.

### For Portfolio Managers
- **Rapid screening**: Identify investment opportunities across 500 stocks.
- **Consistent framework**: Apply same methodology across all holdings.
- **Client reporting**: Visual explanations of portfolio construction logic.
- **Rebalancing automation**: Quickly identify when to adjust allocations.

### For Quantitative Analysts
- **Research foundation**: Starting point for advanced strategies.
- **Factor testing**: Evaluate which metrics drive returns.
- **Strategy iteration**: Easily modify weights and test alternatives.
- **Data pipeline**: Reusable infrastructure for other analyses.

### For Academic Research
- **Teaching tool**: Demonstrate portfolio construction in finance courses.
- **Research baseline**: Compare advanced models against this benchmark.
- **Methodology transparency**: Fully documented, reproducible process.

### Measurable Outcomes
- **Scalability**: Analyze 500 stocks in <30 minutes (vs days manually).
- **Consistency**: 0% human error in score calculations.
- **Repeatability**: Identical results when rerun with same data.
- **Flexibility**: Adjust allocation strategy with 5 lines of code.

## 🚀 Getting Started

### Prerequisites

**1. Install TA-Lib** (required for technical analysis):

**macOS**:
```bash
brew install ta-lib
```

**Linux**:
```bash
wget http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz
tar -xzf ta-lib-0.4.0-src.tar.gz
cd ta-lib/
./configure --prefix=/usr
make
sudo make install
```

**Windows**:
Download and install from: https://www.lfd.uci.edu/~gohlke/pythonlibs/#ta-lib

### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/stock-portfolio-construction.git
cd stock-portfolio-construction

# Install Python dependencies
pip install -r requirements.txt

# Download NLTK data for sentiment analysis
python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords'); nltk.download('wordnet')"
```

### Configuration

**Reddit API Setup** (for sentiment analysis):

1. Create a Reddit app at https://www.reddit.com/prefs/apps.
2. Get your `client_id`, `client_secret`, and `user_agent`.
3. Update `sentimental.ipynb`:

```python
reddit = praw.Reddit(
    client_id='YOUR_CLIENT_ID',
    client_secret='YOUR_CLIENT_SECRET',
    user_agent='YOUR_USER_AGENT'
)
```

### Running the Analysis

Execute notebooks in order:

```bash
# Step 1: Collect fundamental data (~15 minutes)
jupyter notebook fundamental.ipynb

# Step 2: Collect technical data (~5 minutes)
jupyter notebook technical.ipynb

# Step 3: Collect sentiment data (~20 minutes)
jupyter notebook sentimental.ipynb

# Step 4: Generate ratings (~1 minute)
jupyter notebook rating.ipynb

# Step 5: Construct portfolio (~1 minute)
jupyter notebook portfolio.ipynb

# Step 6: Create visualization (~1 minute)
jupyter notebook visualization.ipynb
```

**Total Runtime**: ~45 minutes for complete analysis of 500 stocks.

### Outputs

After running all notebooks, check the `Data/` folder:
```
Data/
├── fundamental.csv      # 490+ stocks × 10 metrics
├── technical.csv        # 490+ stocks × 8 indicators
├── sentimental.csv      # 490+ stocks × 1 sentiment score
├── rating.csv           # 490+ stocks × 4 scores (F/T/S/Rating)
├── analysis.csv         # Full dataset with company metadata
├── portfolio.csv        # 35-45 selected stocks with allocations
└── visualization.png    # 4-panel portfolio construction diagram
```

## 🔧 Customization

### Adjust Portfolio Allocation

Edit `portfolio.ipynb`:

```python
# Change budget
budget = 10000  # €10,000 instead of €5,000

# Change type allocation
target_allocation = {
    'Growth': 0.60,      # 60% instead of 50%
    'Defensive': 0.25,   # 25% instead of 30%
    'Cyclical': 0.15     # 15% instead of 20%
}

# Change sector weights
sector_weights = {
    'Information Technology': 0.50,  # 50% instead of 40%
    # ... modify others
}
```

### Modify Rating Formula

Edit `rating.ipynb`:

```python
# Change final rating weights
df2["Rating"] = (
    df2["Fundamental"] * 0.60 +  # 60% instead of 50%
    df2["Technical"] * 0.30 +    # 30% (unchanged)
    df2["Sentimental"] * 0.10    # 10% instead of 20%
)
```

### Change Technical Indicator Periods

Edit `technical.ipynb`:

```python
# Example: Make indicators more short-term
df["SMA"] = talib.SMA(df["Close"], timeperiod=50)   # 50 instead of 200
df["RSI"] = talib.RSI(df["Close"], timeperiod=14)   # 14 instead of 60
```

### Use Different Sentiment Source

Edit `sentimental.ipynb`:

```python
# Change subreddit
subreddit = reddit.subreddit("stocks")  # Instead of wallstreetbets

# Or combine multiple subreddits
subreddits = ["wallstreetbets", "stocks", "investing"]
```

## 📊 Advanced Usage

### Multi-Period Analysis

Track portfolio changes over time:

```python
# Run quarterly and compare
Q1_portfolio = pd.read_csv("Data/Q1_portfolio.csv")
Q2_portfolio = pd.read_csv("Data/Q2_portfolio.csv")

# Identify stocks added/removed
new_stocks = set(Q2_portfolio['Symbol']) - set(Q1_portfolio['Symbol'])
removed_stocks = set(Q1_portfolio['Symbol']) - set(Q2_portfolio['Symbol'])
```

### Custom Scoring Models

Create your own weighting scheme:

```python
# In rating.ipynb, create alternative ratings
df2["Growth_Focus_Rating"] = (
    df2["Fundamental"] * 0.30 +
    df2["Technical"] * 0.20 +
    df2["Sentimental"] * 0.10 +
    df[["RGR", "EGR"]].mean(axis=1) * 0.40  # Heavy growth weight
)
```

## 🤝 Contributing

**How to Contribute**:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.
