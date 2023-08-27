# Stock News Alert System

Sends news about specific stock via SMS when the closing price of yesterday and the day before yesterday differs by more than 5%

---

To keep track of my stocks by receiving news alert about them, when the closing price of yesterday and the day before yesterday differs by more than 5%. The system is a very early prototype and I have many ideas to extend its. I'm working on realising those ideas.

## version 1
There are 3 parts for this version:
1. Retrieving stock prices from Alpha Vantage through the API.
2. Fetching news related to the stock from Newsdata through the API when specific conditions are met.
3. Sending SMS using the Twilio API to convey information about stock price changes, headlines, and brief descriptions.

I then scheduled the Python code to run on a daily basis using PythonAnywhere

```PYTHON
import requests
from twilio.rest import Client

def calc_percentage_change_closing_price(day, day_minus_one):
    """get day_n's closing price and day_n-1's closing price
    return the percentage changes in closing price (%)"""
    return ((day / day_minus_one) - 1) * 100


def get_news():
    news_endpoint = "https://newsdata.io/api/1/news"
    new_param = {
        "apikey": NEWS_API_KEY,
        "q": STOCK,
        "language": "en",
    }

    news_response = requests.get(news_endpoint, params=new_param)
    news_response.raise_for_status()

    return news_response.json()["results"]


def send_message(title, description, closing_price_percentage_change):
    if closing_price_percentage_change >= 0:
        emoji = "⬆️"
    else:
        emoji = "⬇️"

    client = Client(TWILIO_SID, TWILIO_TOKEN)
    message = client.messages.create(
        body=f"{STOCK}: {emoji}{abs(closing_price_percentage_change):.2f}%\nHeadline: {title}\nBrief: {description}",
        from_=TWILIO_PHONE_NUMBER,
        to=TO_PHONE_NUMBER,
    )
    print(message.sid)

stock_endpoint = "https://www.alphavantage.co/query"
stock_params = {
    "function": "TIME_SERIES_DAILY",
    "symbol": STOCK,
    "apikey": STOCK_API_KEY,
}
stock_response = requests.get(stock_endpoint, params=stock_params)
stock_response.raise_for_status()
stock_data = stock_response.json()["Time Series (Daily)"]

# logic for handle stock_prices dict from alphavantage API
stock_data_sorted_by_date = sorted(stock_data.items(), key=lambda x: x[0], reverse=True)
yesterday_stock_price = float(stock_data_sorted_by_date[0][1]["4. close"])
day_before_yesterday_stock_price = float(stock_data_sorted_by_date[1][1]["4. close"])
closing_price_percentage_change = calc_percentage_change_closing_price(
    yesterday_stock_price, day_before_yesterday_stock_price
)

if abs(closing_price_percentage_change) >= 5:
    # send 3 news about STOCK 
    news_data = get_news()

    index = 0
    counter = 0
    while counter < 3:
        news = news_data[index]
        if not news["title"] or not news["description"]:
            # don't want news without title or description
            index += 1
            continue

        send_message(
            news["title"], news["description"], closing_price_percentage_change
        )
        index += 1
        counter += 1
else:
    print(f"No message sent as condition not met: {closing_price_percentage_change}")

```

Outcome:

<img height="400" alt="Stock_News_Alert_System" src="https://github.com/abc12345d/Markdown_workspace/assets/44512722/27f07be3-f58a-43c8-83a5-c5bcb0701315">

