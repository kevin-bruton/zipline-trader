Alpaca Data Bundle
=====================

| Out of the box, I support Alpaca as a data source for data ingestion.
| If you haven't created an account, start with that: `Alpaca Signup`_.
| Why? You could get price data for free using the Alpaca data API. Free data is hard to get.
| Any other vendor could be added and you are not obligated to use that.
| **I currently only support daily data but free minute data will soon follow.**

How To Use It
-----------------
| Please don't use the old method of zipline ingestion. It doesn't work for this bundle, and
  it will be abandoned. So DON'T DO THIS:

.. code-block:: bash

  zipline ingest -b alpaca_api

| DO THIS INSTEAD:
| To ingest daily data bundle using the alpaca api, you need to follow these steps:
* The bundle is defined in this file: ``zipline/data/bundles/alpaca_api.py`` and you will execute it to ingest.
* There is a method called ``initialize_client()``, it relies on the fact that you define your
  alpaca credentials in a file called ``alpaca.yaml`` in your root directory.

  | A root directory is defined as the directory in which you run your python scripts from.
  | It should look like this:

  .. code-block:: yaml

    key_id: "<YOUR-KEY>"
    secret: "<YOUR-SECRET>"
    base_url: https://paper-api.alpaca.markets
    universe: SP500
  ..
* Selecting your universe - which means: what stocks should be included in the ingested DB. The smaller the universe,
  the faster the ingestion is. Selecting the universe is completely up to you, but I do suggest to start small when
  learning how to use the platform. Supported universes:

  .. code-block:: python

    class Universe(Enum):
        ALL = "ALL"  # The entire ~8000 stocks supported by Alpaca
        SP100 = "S&P 100"
        SP500 = "S&P 500"
        NASDAQ100 = "NASDAQ 100"
  ..

* you need to define your ZIPLINE_ROOT in an environment variable (This is where the
  ingested data will be stored). You need this env variable in for every zipline related script you execute (that
  includes the ingestion process, your research notebooks, and backtests you run) .It should be something like this:

  .. code-block:: yaml

    ZIPLINE_ROOT=~/.zipline
  ..

  | note: Your root directory as defined previously is independent from the ZIPLINE_ROOT directory.
    You could define both in the same location or if you wish to store your DB else where, it's fine too.
  | note: ~/.zipline means your home directory with .zipline directory in it.
  | note: You could set it as an absolute path too. e.g:
  .. code-block:: yaml

    ZIPLINE_ROOT=/opt/project/.zipline
  ..


  | It means you could basically put it anywhere you want as long as you always use that as your zipline root.

  | It also means that different bundles could have different locations.

* By default the bundle ingests 30 days backwards, but you can change that under the
  ``__main__`` section of ``zipline/data/bundles/alpaca_api.py``.
* To ingest the bundle you need to run the ingestion script directly. Run this:

  .. code-block:: bash

    cd zipline_trader/
    python zipline/data/bundles/alpaca_api.py

  ..
| The ingestion process for daily data using Alpaca is extremely fast due to the Alpaca
  API allowing to query 200 equities in one api call.


Notes
))))))))

* You are ready to research, backtest or paper trade using the pipeline functionality.
* You should repeat this process daily since every day you will have new price data.
* This data doesn't include Fundamental data, only price data so we'll need to handle it separately.

.. _`Alpaca Signup` : https://app.alpaca.markets/signup