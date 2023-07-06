# Binance Multi Coin Grid Trader

## About
Binance Multi Coin Grid Trader is a bot, allowing to dynamically create trading grids. It takes the parameters
and calculates for each symbol BUY LIMIT grids evenly (drop and amount). Once deployed, it automatically monitors
 the state of the grid and places SELL LIMIT orders accordingly. 

## Features

- [x] Create limit buy order grid for specific market under set conditions (Grid level distance, grid count,etc.) equally distributed.
  - [x] Grids are created on bot startup if no previous grids in database founds
  - [X] Grids are created based on market manager. If RSI of N minute candles is less that N than grid will be deployed
    - [X] Customized RSI params:
      - [X] Timeframe of OHLCV candels: 
        - [X] Minutes: 1, 5, 15, 30
        - [X] Hours: 1H
      - [X] RSI length as INT
      - [X] RSI value serving as limit for grid creation: RSI value less than N RSI
- [x] Monitors grid orders and once purchase is done, it automatically sets the sell limits based on setup parameters
- [x] Monitors limit sell orders 
- [x] Activities stored in Mongo Database collections for tracking
  - [x] Limit buy order grids
  - [x] limit sell orders
  - [x] completed grid levels
  - [ ] performance calculation
- [ ] Once Grids are completed make new grids once grid is completed
  - [ ] Create new grids if there are free grid slots based on market manager
- [ ] Overall account performance tracking

## Install  

### Clone the repository
```buildoutcfg
git clone https://github.com/AnimusXCASH/BinanceGridTrader.git
```
### Install Project Requirements
```
pip3 install -r requirements.txt
```

### Install MongoDB

### Bot Setup File
Create file into main directory under the name `botSetup.json` and fill in following requirements.

```json
{
    "binancePrivate": "ORW2huYVmHHuHsUAK6rZCA1wAm45sWFVtqHbndjZsm5cuzMfGgcI5v9sUqeK77BG",
    "binancePublic": "WzZ9JgOQZFDVY0rmBYUMFfbZLmfMUtsty3kpkSUu8ncZKwv4os8mHbLoPS5RzDaK",
    "markets": ["BNBUSDT", "MATICUSDT"],  // Список пар, для которых создается сетка (например, ["BNBUSDT", "BTCUSDT"])
    "gridDistance": 0.5,  // Устанавливает расстояние между сетками в 50% (можно изменить, например, на 0.01 для 1%)
    "grids": 4,  // Количество сеток для создания ордеров на покупку (например, установите 10 для создания 10 сеток)
    "base": "bid", // Основа для расчета сетки: bid (цена предложения) или ask (цена спроса)
    "dollarPerCoin": 100, // Количество USDT, выделяемое для каждой пары (например, установите 100 для использования 100 USDT) Это означает, что вы выделяете 100 USDT для торговли каждой валютной парой, и у вас есть 4 сетки. Следовательно, каждый ордер в сетке будет составлять 100/ "grids": 4 = 25 USDT.
    "gain": 0.020, // (Take Profit) Процент выхода для ограничения продажи от отмеченной цены покупки (например, 0.05 для 5%) 
    "monitor": 120, // Количество секунд между повторными проверками сетки (например, установите 300 для проверки каждые 5 минут)
    "stopLimitSellPerc": 0.02,  //  (Stop Loss) Установите процент стоп-лимита на продажу (например, 0.03 для 3%)
    "rsiTimeframe": "15", // Укажите таймфрейм OHLCV для запроса данных (например, "5" для 5 минут или "1H" для 1 часа)
    "rsiLength": 14, // Укажите длину RSI, применяемую к закрытию (например, установите 10 для RSI длиной 10)
    "rsiManager": 47 // Установите предел RSI для развертывания сетки. Сетки не создаются, если значение выше выбранного числа (например, 50)
  }

```

## Run the script from CMD 
```
python main.py
```
or
```
python3 main.py
```
