# Installation

To use this API you need the following installed in your system:

* Python 3.10.4
* websocket-client 1.4.1
* openpyxl 3.0.10

You can get the python installer in the python official website: 
https://www.python.org/downloads/

For installing the other modules you're going to need pip installed in your system, if you don't have it yet, you can find it here:
https://pypi.org/project/pip/

If you already have pip, then you can install the modules either by executing the "install.bat" script or by using the following command line commands:

```
pip install websocket-client==1.4.1
pip install openpyxl==3.0.10
```

# First Steps

To start using the API you first need to login into your account on XTB, here's an example on how to do that:

```python
from API import XTB

API = XTB("ID", "Password")

#your code

API.logout()
```

The example above represents the basic structure for any code using the API.

The "ID" can be found in the right of the account description (REAL or DEMO) in the trading platform.

# Methods Description

## get_AllSymbols

Description: Returns a list of all symbols available for the user.

Parameters: None.

```python
list = API.get_AllSymbols()
```

## get_Candles

Description: Returns a list of dictionarys with the candles information.

If an error occurs, it will return the FALSE boolean value.

Parameters:
* period - required - Period of the candle chart ("M1", "M5", "M15", "M30", "H1", "H4", "D1", "W1", "MN1").
* symbol - required - Symbol ("EURUSD", "EURJPY").
* days - optional - Number of days from the current time to take the candles from.
* hours - optional - Number of hours from the current time to take the candles from.
* minutes - optional - Number of minutes from the current time to take the candles from.
* qty_candles - optional - Number of candles you want to retrieve information of.

```python
list = API.get_Candles("M1", "AUDUSD", qty_candles=10)
```

## get_CandlesRange

Description: Returns a list of dictionarys with the candles information between given start and end dates.

If an error occurs, it will return the FALSE boolean value.

Parameters:
* period - required - Period of the candle chart ("M1", "M5", "M15", "M30", "H1", "H4", "D1", "W1", "MN1").
* symbol - required - Symbol ("EURUSD", "EURJPY").
* start - optional - Start date in the format "month/day/year hours:minutes:seconds" ("05/22/2020 00:00:00").
* end - optional - End date in the format "month/day/year hours:minutes:seconds" ("07/03/2022 12:00:00").
* days - optional - Number of days from the current time to take the candles from.
* hours - optional - Number of hours from the current time to take the candles from.
* minutes - optional - Number of minutes from the current time to take the candles from.
* qty_candles - optional - Number of candles you want to retrieve information of.

```python
list = API.get_CandlesRange("M1", "AUDUSD", start="05/22/2020 00:00:00", end="07/03/2022 12:00:00")
```

## get_ServerTime

Description: Returns current time on trading server, the time in the server is the number of milisseconds from 01/01/1970 00:00:00 until the current moment.

Parameters: None.

```python
time = API.get_ServerTime()
```

## get_Balance

Description: Returns the current balance in the account.

Parameters: None.

```python
balance = API.get_Balance()
```

## get_Margin

Description: Returns expected margin for given symbol and volume. The value is calculated as expected margin value, and therefore might not be perfectly accurate.

Parameters:
* symbol - required - Symbol ("EURUSD", "EURJPY").
* volume - required - Volume (0.01, 0.5, 5.0).

```python
margin = API.get_Margin("AUDCAD", 3.5)
```

## get_Profit

Description: Calculates estimated profit for given deal data.

Parameters:
* open_price - required - Open price for the transaction.
* close_price - required - Close price for the transaction.
* transaction_type - required - 1 for buy and 0 for sell.
* symbol - required - Symbol ("EURUSD", "EURJPY").
* volume - required - Volume (0.01, 0.5, 5.0).

```python
profit = API.get_Profit(0.73750, 0.74000, 1, "AUDUSD", 5.0)
```

## get_Symbol

Description: Returns a dictionary with information about symbol available for the user.

Parameters:
* symbol - required - Symbol ("EURUSD", "EURJPY").

```python
information = API.get_Symbol("EURUSD")
```

## make_Trade

Description: Starts trade transaction. A TRUE value in the status does not means that the transaction was successful, it only means that the requisition was sent to the server without errors. To make sure the transaction was successful, use the check_Trade method.

Parameters:
* symbol - required - Symbol ("EURUSD", "EURJPY").
* cmd - required - Operation code (possible values below).
* transaction_type - required - Transaction type (possible values below).
* volume - required - Volume (0.01, 0.5, 5.0).
* comment - optional - Comment that can be used to locate this specific transaction later.
* order - optional - Order code for closing/modifications.
* sl - optional - Stop loss, can only be used with sell operation.
* tp - optional - Take profit, can only be used with buy operation.
* days - optional - Number of days from the current time for the transaction to expire.
* hours - optional - Number of hours from the current time for the transaction to expire.
* minutes - optional - Number of minutes from the current time for the transaction to expire.

Values cmd:
* BUY - 0 - buy.
* SELL - 1 - sell.
* BUY_LIMIT - 2 - buy limit.
* SELL_LIMIT - 3 - sell limit.
* BUY_STOP - 4 - buy stop.
* SELL_STOP - 5 - sell stop.

Values transaction_type:
* OPEN - 0 - order open.
* PENDING - 1 - DOES NOT WORK.
* CLOSE - 2 - order close.
* MODIFY - 3 - order modify.
* DELETE - 4 - order delete.

```python
status, order_code = API.make_Trade("USDCAD", 0, 0, 0.1, comment="000001")
```

## check_Trade

Description: Returns current transaction status.

Parameters:
* order - required - Order code.

Values status:
* ERROR - 0 - error.
* PENDING - 1 - pending.
* ACCEPTED - 3 - The transaction has been executed successfully.
* REJECTED - 4 - The transaction has been rejected.

```python
status = API.check_Trade(419113948)
```

## get_History

Description: Returns list of user's trades closed within specified period of time.

Parameters:
* start - optional - Start date in the format "month/day/year hours:minutes:seconds" ("05/22/2020 00:00:00"), default is one month from the current moment.
* end - optional - End date in the format "month/day/year hours:minutes:seconds" ("07/03/2022 12:00:00"), default is the current moment.
* days - optional - Number of days from the end date to take the transactions from.
* hours - optional - Number of hours from the end date to take the transactions from.
* minutes - optional - Number of minutes from the end date to take the transactions from.

```python
list = API.get_History()
```

## ping

Description: Regularly calling this function is enough to refresh the internal state of all the components in the system. It is recommended that any application that does not execute other commands, should call this command at least once every 10 minutes.

Parameters: None.

```python
status = API.ping()
```

## candles_to_excel

Description: Creates an excel file and stores the candles returned from the get_Candles or get_CandlesRange methods.

Parameters:
* candles - required - The candles returned from the get_Candles or get_CandlesRange methods.
* address - required - Address where to save the excel file.
* name - required - Name for the file followed by the extension (.xlsx).

```python
status = API.candles_to_excel(candles, "C:\Users\User\Desktop\", "Candles.xlsx")
```

## get_candles_from_excel

Description: Open and reads the excel file where the candles are stored and then obtain and return de candles.

Parameters:
* address - required - Address where the excel file is located.
* name - required - Name of the file followed by the extension (.xlsx).

```python
candles = API.get_candles_from_excel("C:\Users\User\Desktop\", "Candles.xlsx")
```
