# CoinTK
## Bitcoin Trading Algorithm Backtesting and Analysis Toolkit

CoinTK -- An open-sourced framework for rapid prototyping and testing of Bitcoin trading strategies. Also check out [BitBox Server](https://github.com/CoinTK/BitBox-Server), a webserver  built on CoinTK for backtesting and dry running prototype strategies (remote control coming soon!), and [BitBox iOS](https://github.com/CoinTK/BitBox), an iOS APP integrated with the BitBox server for monitoring, analyzing, visualizing, and (soon) initializing backtests.

CoinTK keeps humans in the loop by providing them with the analysis and visualizations they need to make informed decisions about the trading algorithms they use.

---

# Getting Started

1. Make sure `python3` and `python3-pip` are installed:
    ```
    sudo apt install python3 python3-pip
    ```

2. Clone and install `cointk` from pip
    ```
    sudo pip3 install cointk
    ```

    Or, if you prefer, install manually from this repository:
    
    ```
    cd && git clone https://github.com/cointk/cointk.git
    cd cointk
    sudo pip3 install .
    ```

3. Initialize `cointk`
    ```
    cd && python3 -c 'import cointk.init'
    ```

4. Start writing strategies!  As an example, try backtesting the naive
strategy included in cointk
    ```
    cd && mkdir -p plots histories
    ```

    Create `~/naive.py` with the following contents:

    ```
    # ~/naive.py
    from cointk.backtest import backtest
    from cointk.strategies import NaiveStrategy

    strategy = NaiveStrategy()
    backtest(strategy)
    ```

    Run the script:

    ```
    python3 naive.py
    ```

    You should see something like this pop up in a browser window:

    ![Naive Backtest Output](plots/naive_plot.png)

    From here, you can play around with different strategies and testing parameters via scripts in `backtests`, or start thinking about making your own [strategy](#creating-your-own-strategies).

    Happy developing!


# Example strategies
We've implemented a few example strategies and backtested them on the automatically downloaded coinbase to USD dataset, with many more to come.
* [Naive](cointk/strategies/naive.py): Very straightforwardly buy when more than a certain threshold of the previous `n` timesteps have seen an increase in price, sell when more than a certain threshold have seen an increase.
* [Reverse Naive](cointk/strategies/naive_reverse.py): Amazingly, on some test sets, doing the exact opposite of what is described above performs better. This may serve to temper one's expectations with respect to trading algorithms–something completely crazy might work well on one particular dataset.
    ![](plots/naive_reverse.png)
* [Random](cointk/strategies/simple_random.py): Similar to the reverse naive in purpose, we've included a random strategy that occasionally performs well on certain subsets of the training data.
    ![](plots/simple_random.png)
* [Exponential Moving Average (EMA)](cointk/strategies/ema.py): Here we use an a simple exponential moving average to approximate price trends. If the trend is going up (and crosses the current price) then sell, and if the trend is going down (and crosses the current price), then buy.
    ![](plots/ema.png)



# File structures

* `cointk/` contains most of the algorithmic work

  * `strategies/` contains different buying/selling strategies, which is just a decision framework based on the given state of price/quantity and past histories

    * `prescient/` contains strategies that have access to perfect information, i.e. all historical and future data. These are only useful for a Machine Learning extension we will build in the future, which we hope to train to model such a prescient strategy *without* having perfect informaiton.

* `example_backtests/` tests our sample strategies running on historical data, so you can evaluate performance had you ran this strategy since the beginning

* `plots/` contain plots generated locally by `plotly` -- such as when you run [backtest.py](cointk/backtest.py).

* `trainings/` contain support files for `cointk/strategies/prescient`, which will be flushed out with


# Creating your own strategies

To create your own strategy, create a class similar to one of the sample strategies given: [Naive](cointk/strategies/naive.py), [Reverse Naive](cointk/strategies/naive_reverse.py), [Random](cointk/strategies/simple_random.py), and [Exponential Moving Averages](cointk/strategies/ema.py). It should inherit the `Strategy` class (defined [here](cointk/strategies/core.py)) and have a
```
	gen_order(self, ts, price, qty, funds, balance):
```
function that decides, given the tuple (ts, price, qty) and any past histories stored in the `Strategy` class, whether to buy or sell.

# Contributing

Like what you see? Check out our [contributing guide](https://github.com/CoinTK/BitBox-Server/blob/master/CONTRIBUTING.md) to see how you can help!

# License

CoinTK is [MIT licensed](http://mit-license.org/).
