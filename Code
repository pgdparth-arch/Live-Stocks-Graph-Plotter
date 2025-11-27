import tkinter as tk
from tkinter import ttk
import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg

datasheet = r"D:\Code\Visual Studio 2022\Code Snippets\XLSX\Book1.xlsx"
#location of datasheet
stocks= ["NVDA","TSLA","AAPL","AMZN","MSFT","META","GOOGL","AMD","PLTR","NFLX",
 "INTC","F","RIVN","LCID","COIN","BABA","SHOP","CRWD","PYPL",
 "CRM","ORCL","SPCE","NIO","MARA","RIOT","JPM"]
#stocks list
with pd.ExcelWriter(datasheet, engine="openpyxl",mode="w") as writer:
    #excelwriter for many sheets, openpyxl for read and write the datasheet, mode=w means write or overwrite the datasheet
    for s in stocks:
        chosen_stock=yf.Ticker(s)
        stock_prev_data= chosen_stock.history(period="2y")
        #taking all data till 1 year
        stock_prev_data.index = stock_prev_data.index.tz_localize(None)
        #remove timezone as excel cant take timezone data and our index has time zones
        stock_prev_data["MA20"] = stock_prev_data["Close"].rolling(window=20).mean()
        #creating 20 days moving average
        stock_prev_data["MA50"] = stock_prev_data["Close"].rolling(window=50).mean()
        #creating 50 days moving average
        stock_prev_data = stock_prev_data.drop(["Dividends", "Stock Splits"], axis=1)
        #cleaned data by removing dividents and stocksplits columns
        stock_prev_data.to_excel(writer, sheet_name=s)
        #save each stock to its own sheet
print("Data saved successfully!")
#sign of successful data saving in excel



def show_graph(stocks):
    data_stock= pd.read_excel(datasheet, sheet_name=stocks)
    #opening data of asked stock from datasheet
    data_stock["Date"] = pd.to_datetime(data_stock["Date"])
    data_stock.set_index("Date", inplace=True)
    fig, ax1 = plt.subplots(figsize=(16,10),dpi=120)
    #figure for size of canvas, ax1 for plotting graph
    ax1.plot(data_stock.index, data_stock["Open"], label="Open Price", color="violet", linewidth=0.5)
    ax1.plot(data_stock.index, data_stock["Close"], label="Close Price", color="indigo", linewidth=0.5)
    ax1.plot(data_stock.index, data_stock["High"], label="Highest Price", color="blue", linewidth=0.5)
    ax1.plot(data_stock.index, data_stock["Low"], label="Lowest Price", color="green", linewidth=0.5)
    ax1.plot(data_stock.index, data_stock["MA20"], label="MA20", color="orange", linewidth=0.5)
    ax1.plot(data_stock.index, data_stock["MA50"], label="MA50", color="red", linewidth=0.5)
    #graphs plotted in figure

    ax1.set_xlabel("Date")
    ax1.set_ylabel("Price (USD)")
    ax1.legend(loc="upper left")
    plt.title(f"{stocks} Price History with MA20 and MA50")

    # Clear old graph
    for widget in frame_graph.winfo_children():
        widget.destroy()

    # Put matplotlib graph into Tkinter
    canvas = FigureCanvasTkAgg(fig, master=frame_graph)
    canvas.draw()
    canvas.get_tk_widget().pack()

    plt.close(fig)


# ---- Tkinter Window ----
root = tk.Tk()
root.title("Stock Viewer")
root.geometry("1920x1200")

# Left panel (buttons)
frame_left = tk.Frame(root, padx=15, pady=15)
frame_left.pack(side=tk.LEFT, fill="y")

# Right panel (graph)
frame_graph = tk.Frame(root)
frame_graph.pack(side=tk.RIGHT, expand=True, fill="both")

# Create buttons
for s in stocks:
    btn = ttk.Button(frame_left, text=s, width=20, command=lambda x=s: show_graph(x))
    btn.pack(pady=5)

root.mainloop()
