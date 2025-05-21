# Automating Data Retrieval from Google News with RPA

This README file provides an overview of the Python script used to automate the retrieval of news data from Google News using the RPA (Robotic Process Automation) library. This script fetches news related to specified symbols from a pandas DataFrame, analyzes the sentiment, and stores the results in a new column within the DataFrame. The script also saves the modified DataFrame to a CSV file.

**Note**: The symbols used in the example pandas DataFrame were obtained from the [EODData website](https://eoddata.com/stocklist/FOREX/U.htm).


## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Closing Remarks](#closing-remarks)

## Introduction <a name="introduction"></a>

This Python script leverages the RPA library to automate the process of retrieving news data from Google News and analyzing its sentiment. It operates on a pandas DataFrame containing a column with symbols of interest and adds a new column to store the fetched news data. The script also saves the modified DataFrame to a CSV file.

## Prerequisites <a name="prerequisites"></a>

**OpenJDK from Amazon**

You can download and install OpenJDK from the [Amazon Corretto website](https://aws.amazon.com/corretto/).

**Chrome WebDriver**

To use the Chrome WebDriver, follow these steps:

- Download the Chrome WebDriver compatible with your Chrome browser version from the [ChromeDriver download page](https://chromedriver.chromium.org/downloads).
- Ensure the downloaded WebDriver executable is in your system's PATH or provide the WebDriver path in your script for proper functionality.
- These installations are prerequisites for running the script successfully.

**TagUI**

To use TagUI, you'll need to install it. Follow the [TagUI installation instructions](https://tagui.readthedocs.io/en/latest/setup.html) to set up TagUI on your system.

**Note:** During the automation process, we used [Selectors Hub](https://selectorshub.com/) to inspect and select elements on web pages.

Before using this script, ensure that you have the following prerequisites in place:

- Python installed on your system.
- Required Python libraries: `rpa` and `pandas`. You can install them using pip:

```bash
pip install rpa pandas
```
# Getting Started <a name="getting-started"></a>
**Importing Libraries:** The script starts by importing the necessary libraries.
```python
import rpa as r
import pandas as pd
```
**Initializing Chrome:** It initializes a Chrome browser session using RPA.
```python
r.init(chrome_browser=True, turbo_mode=True, visual_automation=True)
```
**Reading the DataFrame**: The script reads data from a pandas DataFrame containing a 'symbol' column.
```python
# Reading from the pandas dataframe
news = pd.read_csv("news-symbol.csv", encoding='utf-8')
```
**Adding a New Column:** A new column called 'fetched_data' is added to the 'news' DataFrame to store the fetched news data.
```python
# Add a new column to store the data
news['fetched_data'] = ''
```
**Iterating through Symbols:** The script iterates through the 'symbols' column of the 'news'
```python
for index, row in news.iterrows():
    symbol = row['symbol']  # Assuming 'symbol' is the name of the column with symbols
```
**Fetching News Data:** For each symbol, it performs the following actions:
Navigates to the Google News website.
Constructs a prompt based on the symbol.
Types the prompt and sends an "Enter" key press.
Reads and stores the fetched news data.
```python
    r.url("https://bard.google.com/")
    r.keyboard("[enter]")
    
    # Construct the prompt using the symbol
    prompt = f"{symbol} latest news and tell me the impact of that news if it's positive or negative or neutral"
    
    r.type("//textarea[@placeholder='Enter a prompt here']", prompt)
    r.keyboard("[enter]")
    r.wait()
    
    data = r.read("//div[@dir='ltr']")
    # Remove newline characters from the fetched data
    data = data.replace('\n', ' ')
    news.at[index, 'fetched_data'] = data  # Assign the fetched data to the 'fetched_data' column
```
**Closing the RPA Browser:** After fetching news data for all symbols, the RPA browser is closed.
```python
# Close the RPA browser
r.close()
```
**Saving the Modified DataFrame:** The 'news' DataFrame, now containing a new column 'fetched_data' with the fetched data, is saved to a CSV file.
```python
# Save the modified DataFrame to a CSV file
news.to_csv("modified.csv", index=False)
```
## Usage <a name="usage"></a>
To use this script, follow these steps:

Ensure you have the required prerequisites (Python, rpa, pandas) installed.

Create a CSV file named "news-symbol.csv" with a 'symbol' column containing the symbols you want to fetch news for.

Run the script, and it will automate the process of fetching and storing news data for the specified symbols.

## Closing Remarks <a name="closing-remarks"></a>
This script simplifies the process of gathering news data for specific symbols and analyzing sentiment using RPA. It can be a valuable tool for automating data collection tasks in financial or market analysis.
