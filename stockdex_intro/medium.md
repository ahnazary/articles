<!-- First medium article to publish about stockdex -->

# Stockdex: Python package for stock data extraction and visualization

When it comes to stock market analysis, data extraction and visualization are two key steps. Few tools tools and packages are out there to help in this context. In python ecosystem, [yfinance](https://github.com/ranaroussi/yfinance) is a popular package mainly helping with raw data extraction. However, it has some drawbacks mainly due to limitations in yahoo finance API. Some of these limitations are:

1. Historical data extraction limitaion: yfinance allows to extract historical data for a maximum of 5 years or quarters for annual or quarterly data respectively. Older data is not available.
2. Lack of visualization support: yfinance does not provide built-in support for data visualization. The output data is pandas dataframe. 
3. Being single source: yfinance is solely based on yahoo finance API. In case of of a field or asset not provided by yahoo finance API, yfinance will not be able to extract data for that field or asset.

[Stockdex](https://github.com/ahnazary/stockdex) is an alternative python package that addresses some of these limitations. It has the following benefits over yfinance:

1. Various data sources: Stockdex uses multiple data sources to extract data, including yahoo finance API and website, digrin, macrotrends and justETF websites.
2. Availability of historical data: Although yahoo does not provide data for older than 5 years, stockdex uses other data sources that provide data for older than 5 years, for example, digrin provides dividend data for the entire history of most stocks.
3. Built-in visualization support: Using [plotly](https://github.com/plotly/plotly.py) and [dash](https://github.com/plotly/dash), ploting raw data in form of cahrts or dashboards is possible.

In the next sections, we go through the installation and usage of stockdex.

## Installation

Stockdex can be installed using pip:

```bash
pip install stockdex --upgrade --no-cache-dir
```

## Usage

Main entry point of stockdex is `Ticker` class. After an instance of `Ticker` is created, different methods can be called to extract data. Each method is prefixed with the data source it uses. For example, `yahoo_api_price` method uses yahoo finance API to extract price data, or 
`digrin_stock_splits` method uses digrin website to extract stock splits data. Sectiosn below show some examples of how to use stockdex, categorized by data source.

### Yahoo finance API


```python
from stockdex import Ticker
from datetime import datetime

ticker = Ticker(ticker="AAPL")

ticker = Ticker(ticker="AAPL")

# Price data (use range and dataGranularity to make range and granularity more specific)
price = ticker.yahoo_api_price(range='1y', dataGranularity='1d')

# Current trading period of the stock (pre-market, regular, post-market trading periods)
current_trading_period = ticker.yahoo_api_current_trading_period

# Fundamental data (use frequency, format, period1 and period2 to fine-tune the returned data)
income_statement = ticker.yahoo_api_income_statement(frequency='quarterly')
cash_flow = ticker.yahoo_api_cash_flow(format='raw')
balance_sheet = ticker.yahoo_api_balance_sheet(period1=datetime(2020, 1, 1))
financials = ticker.yahoo_api_financials(period1=datetime(2022, 1, 1), period2=datetime.today())

print(price, current_trading_period, income_statement, cash_flow, balance_sheet, financials)
```