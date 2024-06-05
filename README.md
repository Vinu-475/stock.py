# stock.py
import yfinance as yf
import datetime

class Stock:
    def __init__(self, symbol, shares):
        self.symbol = symbol.upper()
        self.shares = shares
        self.price = 0
        self.value = 0

    def update_price(self):
        stock_data = yf.Ticker(self.symbol)
        self.price = stock_data.history(period="1d")['Close'][0]
        self.value = self.price * self.shares

class Portfolio:
    def __init__(self):
        self.stocks = []

    def add_stock(self, symbol, shares):
        stock = Stock(symbol, shares)
        self.stocks.append(stock)

    def update_prices(self):
        for stock in self.stocks:
            stock.update_price()

    def total_value(self):
        total = 0
        for stock in self.stocks:
            total += stock.value
        return total

    def display_portfolio(self):
        print(f"{'Symbol':<10}{'Shares':<10}{'Price':<10}{'Value':<10}")
        print("="*40)
        for stock in self.stocks:
            print(f"{stock.symbol:<10}{stock.shares:<10}{stock.price:<10.2f}{stock.value:<10.2f}")
        print("="*40)
        print(f"{'Total Value:':<30}{self.total_value():<10.2f}")

def main():
    portfolio = Portfolio()
    portfolio.add_stock('AAPL', 10)
    portfolio.add_stock('GOOGL', 5)
    portfolio.add_stock('TSLA', 7)

    portfolio.update_prices()
    portfolio.display_portfolio()
if __name__ == "__main__":
    main()    
