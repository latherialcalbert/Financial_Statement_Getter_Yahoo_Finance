# Yahoo Finance S&P 500 Financial Data Scraper

This repository contains a Python script that uses the Yahoo Finance API to fetch and save financial statement data (balance sheets) for all the S&P 500 companies. The data is retrieved for the last 4 years, transposed, and saved as CSV files for easy access and analysis.

## Features

- Fetches the S&P 500 tickers from the [Wikipedia page](https://en.wikipedia.org/wiki/List_of_S%26P_500_companies).
- Retrieves balance sheet data for each ticker using the `yfinance` library.
- Transposes the financial data for better readability.
- Saves the transposed balance sheet data to CSV files.
- Ensures that already processed tickers are not fetched again.

## Prerequisites

Before running the script, ensure that you have the following libraries installed:

- `yfinance`: For fetching financial data from Yahoo Finance.
- `pandas`: For data manipulation and saving data to CSV.
- `requests`: For making HTTP requests to the Wikipedia page.

You can install these libraries using `pip`:

```bash
pip install yfinance pandas requests
```

## Script Explanation

### 1. **Fetching S&P 500 Tickers**

The script fetches the list of S&P 500 tickers from the Wikipedia page. The tickers are extracted using the `pandas` library to read the HTML table.

```python
url = "https://en.wikipedia.org/wiki/List_of_S%26P_500_companies"
response = requests.get(url)
tables = pd.read_html(response.text)
sp500_table = tables[0]
sp500_tickers = sp500_table['Symbol'].tolist()
```

### 2. **Retrieving Financial Data**

For each ticker, the script retrieves the balance sheet data from Yahoo Finance using `yfinance`. The financial data is then transposed to make it easier to analyze.

```python
financials = yf.Ticker(ticker).balance_sheet
transposed_financials = financials.T
```

### 3. **Saving Data**

The transposed financial data is saved to a CSV file named with the ticker symbol in the `balancesheets` directory.

```python
csv_filename = f'balancesheets/balance_sheet_{ticker}.csv'
transposed_financials.to_csv(csv_filename, index=True)
```

### 4. **Avoiding Redundancy**

The script ensures that data for each ticker is only retrieved and saved once. If the data for a ticker has already been processed, it skips that ticker.

```python
saved_tickers = set()
if ticker in saved_tickers:
    print(f'Income statement data for {ticker} already saved.')
    continue
```

## Directory Structure

The project will create a `balancesheets` folder to store the CSV files with the balance sheet data for each S&P 500 company. The folder will be created automatically if it doesn't already exist.

```
balancesheets/
    balance_sheet_AAPL.csv
    balance_sheet_MSFT.csv
    ...
```

## Usage

1. Clone this repository to your local machine:

```bash
git clone https://github.com/your-username/financial-data-scraper.git
cd financial-data-scraper
```

2. Run the script:

```bash
python financial_data_scraper.py
```

This will fetch the balance sheet data for all S&P 500 companies and save them as CSV files in the `balancesheets` directory.

## Notes

- The script currently retrieves the balance sheet data, but you can modify it to retrieve other types of financial data (e.g., income statement or cash flow statement) as needed.
- The script may take some time to run as it processes each S&P 500 ticker.

## License

This project is open-source and available under the MIT License. See the [LICENSE](LICENSE) file for more details.

