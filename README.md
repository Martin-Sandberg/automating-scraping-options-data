# automating-scraping-options-data
Script that uses Selenium to webscrape options transaction data from Nasdaq

I created this script to scrape options trading data on several underlying stocks in the Swedish market.

Using Selenium the script scrapes all option trades on the underlying stocks collecting the following information: option type (put/call), date and time of transaction, option price, strike price, strike date, volume, stock price of underlying at the time of the transaction, as well as total volume of call and put options traded for the underlying stock.

The webscraper begins on: http://www.nasdaqomxnordic.com/optionsandfutures, which shows all the stocks for which nasdaq offers trading and clearing in Swedish, Danish, Finnish and Norwegian options and futures.

The only parts of the script that needs to be hardcoded are the xpaths for each of the individual stocks on the previosly mentioned home page as well as the url for each stock's nasdaq page (which will be used to get stock price data).

The first part of the script, the options_scraping_data function, goes to each stock's options page and:

- scrapes the total amount of call and put options traded for the day
- loops through all options and saves all of the options that have been traded during the day ( volume != 0 )
- It then goes to the page of all of the traded options and scrapes the time, price, and volume of all trades for the day from a table
    - However, the table displaying all the trades doesn't load if more than one trade has been done for the same instrument. If the table doesn't load the webscraper gets the price and time of the transactions from a dynamic graph (but misses volume data...)


The second part of the script, the scraping_stock_prices function, goes to each stock's page and gets the stock price of the underlying for each of the recorded transactions from a dynamic price graph. The script gets the price of the underlying stock at the exact time, or just before, the option trade took place.

The output of the script is two dataframes:

all_trades_df: all the option trades on the underlying stocks, and
tot_vol_df: the total volume of call and put options traded on the underlying

I then personally save all the data to an excel file with the sheets named after the current date.

I run this script every week day through crontab after the swedish market closes, so I've also included an email alert that sends a text message to my phone when the script is done running and the data have been collected.

Below are some articles/videos I found really helpful writing this code:
- https://medium.com/p/2bc20a9c7f5c
  - "Scraping Interactive Line Charts with Python"
