## 股票演示程序 `stock_demo.py`

```python
# file stock_demo.py ----------------------------------------
from stock_plot_v4 import *
menu_str = ('Menu choices:\n' +
'0. Exit\n' +
'1. Simple Plot of Prices\n' +
'2. Plot of Daily High/Low Prices\n' +
'3. Plot of Price with Volume Subplot\n' +
'4. Price plus Moving Average\n')
prompt_msg = 'Enter stock symbol (ENTER to exit): '
def main():
    while True:
        # Prompt and re-prompt user until a valid stock
        # symbol is entered OR user exits.
        try:
            s = input(prompt_msg)
            s = s.strip()
            if not s:         # On empty string, exit.
                return
            stock_df = load_stock(s)
            n = int(input(menu_str + 'Input choice: '))
            if n < 0 or n > 4:
                n = 0
            if n == 0:
                return
            fn = [do_simple_plot, do_highlow_plot,
                  do_volume_subplot, do_movingavg_plot][n-1]
            fn(stock_df, s)
except:
    print('Couldn\'t find stock. Re-try.')

main()
```
