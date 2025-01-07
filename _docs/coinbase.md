# Coinbase

<!-- WARNING: THIS FILE WAS AUTOGENERATED! DO NOT EDIT! -->

Manages the coinbase data download and processing. It is important to
note that the Coinbase API might change from time to time, and the
functions might need to be updated. You can find the documentation here:
https://docs.cdp.coinbase.com/exchange/reference/exchangerestapi_getproductcandles.

Notice also that there are limits of what one can download. See the
documentation for more details. In particular:
https://docs.cdp.coinbase.com/exchange/docs/rest-rate-limits

\*\* Finally, datetime columns are in UTC. \*\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L16"
target="_blank" style="float:right; font-size:smaller">source</a>

### retrieve_coinbase_price

>      retrieve_coinbase_price (pair='BTC-USD', time_interval=3600,
>                               end_date='2025-01-07T14:19:15Z',
>                               start_date='2025-01-05T14:19:15Z')

\*Retrieves historical price data from Coinbase for a given trading pair
and time interval.

Makes a GET request to the Coinbase candles endpoint to fetch OHLCV
(Open, High, Low, Close, Volume) data for the specified trading pair and
time period.

Args: pair (str, optional): Trading pair symbol (e.g. “BTC-USD”).
Defaults to “BTC-USD”. time_interval (int, optional): Candle interval in
seconds. Defaults to 3600 (1 hour). end_date (str, optional): End
datetime in ISO 8601 format. Defaults to current UTC time. start_date
(str, optional): Start datetime in ISO 8601 format. Defaults to 2 days
before end_date.

Returns: pandas.DataFrame: DataFrame containing the OHLCV data with
columns: - datetime: Timestamp for the candle (UTC) - low: Lowest traded
price in the interval - high: Highest traded price in the interval  
- open: Opening price of the interval - close: Closing price of the
interval - volume: Trading volume in the interval - pair: Trading pair
symbol Returns None if the API request fails or returns no data.

Notes: - Limited to 300 requests per hour by the Coinbase API - Maximum
of 200 candles can be retrieved per request - Attempting to exceed these
limits will result in an error and return None - All datetime values are
in UTC timezone\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L68"
target="_blank" style="float:right; font-size:smaller">source</a>

### coinbase_tokens

>      coinbase_tokens ()

\*Retrieves all available trading pairs from the Coinbase Exchange API.

Makes a GET request to the Coinbase products endpoint to fetch
information about all trading pairs available on the exchange.

Returns: pandas.DataFrame: DataFrame containing information about all
trading pairs with columns: - id: Trading pair ID (e.g. ‘BTC-USD’) -
base_currency: The cryptocurrency being traded (e.g. ‘BTC’) -
quote_currency: The currency used for pricing (e.g. ‘USD’) -
quote_increment: Minimum price increment - base_increment: Minimum
quantity increment - display_name: Human readable name of the trading
pair - status: Trading status of the pair And other metadata columns
provided by the Coinbase API\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L93"
target="_blank" style="float:right; font-size:smaller">source</a>

### coinbase_usd_tokens

>      coinbase_usd_tokens ()

\*Retrieves all trading pairs from Coinbase that have USD as the quote
currency.

Returns: pandas.DataFrame: DataFrame containing information about USD
trading pairs with columns: - id: Trading pair ID (e.g. ‘BTC-USD’) -
base_currency: The cryptocurrency being traded (e.g. ‘BTC’) -
quote_currency: Always ‘USD’ for this filtered dataset -
quote_increment: Minimum price increment - base_increment: Minimum
quantity increment - display_name: Human readable name of the trading
pair - status: Trading status of the pair And other metadata columns
provided by the Coinbase API\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L113"
target="_blank" style="float:right; font-size:smaller">source</a>

### coinbase_price_history

>      coinbase_price_history (pair='BTC-USD', start_date='2024-01-01',
>                              end_date='2024-05-01', time_interval=3600,
>                              max_pull=250, verbose=False)

\*Downloads historical price data for a cryptocurrency pair from
Coinbase, handling rate limits.

Args: pair (str): Trading pair symbol (e.g. ‘BTC-USD’). Defaults to
‘BTC-USD’. start_date (str): Start date in ‘YYYY-MM-DD’ format. Defaults
to ‘2024-01-01’. end_date (str): End date in ‘YYYY-MM-DD’ format.
Defaults to ‘2024-05-01’. time_interval (int): Time interval between
candles in seconds. Defaults to 3600 (1 hour). max_pull (int): Maximum
number of candles per API request. Defaults to 250. verbose (bool): If
True, prints progress messages. Defaults to False.

Returns: pandas.DataFrame: DataFrame containing price history with
columns: - datetime: Timestamp of the candle - low: Lowest price during
the interval - high: Highest price during the interval  
- open: Opening price of the interval - close: Closing price of the
interval - volume: Trading volume during the interval - pair: Trading
pair symbol

Raises: ValueError: If end_date is earlier than start_date

The function handles Coinbase’s API limitations by automatically
splitting requests into smaller chunks if the date range would exceed
the maximum allowed candles per request.\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L171"
target="_blank" style="float:right; font-size:smaller">source</a>

### save_file

>      save_file (df, folder_path, file_name, type='csv')

\*Save a pandas DataFrame to a file in either CSV or Parquet format.

Args: df (pandas.DataFrame): The DataFrame to save folder_path (str):
Directory path where the file will be saved file_name (str): Name of the
file without extension type (str, optional): File format - either “csv”
or “parquet”. Defaults to “csv”

The function saves the DataFrame to the specified path, handling the
file extension automatically. For CSV files, the index is not saved. For
Parquet files, default Parquet settings are used.\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L192"
target="_blank" style="float:right; font-size:smaller">source</a>

### coinbase_to_file

>      coinbase_to_file (folder_path='../data/coinbase', token_list=['REQ-USD',
>                        'GUSD-USD', 'MOVE-USD', 'TRIBE-USD', 'YFII-USD', 'MLN-
>                        USD', 'MUSE-USD', 'GODS-USD', 'GTC-USD', 'YFI-USD',
>                        'PEPE-USD', 'MUSD-USD', 'ALICE-USD', 'AAVE-USD', 'PRQ-
>                        USD', 'ARB-USD', 'ACS-USD', 'XLM-USD', 'NEAR-USD',
>                        'MOBILE-USD', 'VET-USD', 'AURORA-USD', 'TVK-USD',
>                        'OOKI-USD', 'RLY-USD', 'GFI-USD', 'AXL-USD', 'WELL-
>                        USD', 'FLOW-USD', 'MONA-USD', 'FOX-USD', 'PRO-USD',
>                        'ONDO-USD', 'CHZ-USD', 'EGLD-USD', 'ZK-USD', 'AKT-USD',
>                        'TIME-USD', 'ZETA-USD', 'GMT-USD', 'ARKM-USD', 'LIT-
>                        USD', 'DRIFT-USD', 'JASMY-USD', 'CRO-USD', 'ELA-USD',
>                        'ZETACHAIN-USD', 'MTL-USD', 'CELR-USD', 'ALEPH-USD',
>                        'QUICK-USD', 'FORTH-USD', 'PIRATE-USD', 'OXT-USD',
>                        'ANT-USD', 'AUDIO-USD', 'A8-USD', 'MOODENG-USD',
>                        'WLUNA-USD', 'BLAST-USD', 'POL-USD', 'MANA-USD',
>                        'RONIN-USD', 'PLU-USD', 'SUI-USD', 'FET-USD', 'FARM-
>                        USD', 'MNDE-USD', 'EOS-USD', 'BCH-USD', 'DOGE-USD',
>                        'HIGH-USD', 'ZRO-USD', 'SUPER-USD', 'CRV-USD', 'BOND-
>                        USD', 'STG-USD', 'TONE-USD', 'DAR-USD', 'RNDR-USD',
>                        'FORT-USD', 'GLM-USD', 'MOG-USD', 'APT-USD', 'RAD-USD',
>                        'EIGEN-USD', 'SYLO-USD', 'PNG-USD', 'KSM-USD', 'BAT-
>                        USD', 'SWELL-USD', 'LRDS-USD', 'WBTC-USD', 'AGLD-USD',
>                        'LPT-USD', 'RBN-USD', 'MASK-USD', 'HBAR-USD', 'G-USD',
>                        'SOL-USD', 'AVT-USD', 'SAND-USD', 'DASH-USD', 'BONK-
>                        USD', 'FIL-USD', 'ADA-USD', 'NEON-USD', 'XYO-USD',
>                        'GYEN-USD', 'ORN-USD', 'CVC-USD', 'TNSR-USD', 'BAND-
>                        USD', 'ALCX-USD', 'ZEN-USD', 'OGN-USD', 'COTI-USD',
>                        'PRCL-USD', 'GST-USD', 'API3-USD', 'SEI-USD', 'BOBA-
>                        USD', 'AUCTION-USD', 'GAL-USD', 'HOPR-USD', 'VGX-USD',
>                        'SHDW-USD', 'UST-USD', 'MAGIC-USD', 'TRAC-USD', 'GIGA-
>                        USD', 'IOTX-USD', 'RLC-USD', 'ENJ-USD', 'JTO-USD', 'IO-
>                        USD', 'DESO-USD', 'REP-USD', 'PLA-USD', 'PYR-USD',
>                        'XCN-USD', 'POLY-USD', 'DDX-USD', 'SD-USD', 'ORCA-USD',
>                        'DEXT-USD', 'POLS-USD', 'REN-USD', 'MEDIA-USD', 'MIR-
>                        USD', 'MSOL-USD', 'ICP-USD', 'PYUSD-USD', 'MATH-USD',
>                        'COW-USD', 'LOKA-USD', 'VTHO-USD', 'STRK-USD', 'AMP-
>                        USD', 'POND-USD', 'METIS-USD', 'OMG-USD', 'NU-USD',
>                        'ILV-USD', 'UNI-USD', 'MXC-USD', 'NKN-USD', 'LINK-USD',
>                        'CTSI-USD', 'LRC-USD', 'SPELL-USD', 'STORJ-USD', 'ASM-
>                        USD', 'JUP-USD', 'ABT-USD', 'NMR-USD', 'DEGEN-USD',
>                        'ARPA-USD', 'TURBO-USD', 'GNO-USD', 'LCX-USD', 'SHIB-
>                        USD', 'USDT-USD', 'AXS-USD', 'BNT-USD', 'DAI-USD',
>                        'UNFI-USD', 'CORECHAIN-USD', 'INJ-USD', 'ACH-USD',
>                        'STX-USD', 'RGT-USD', 'SNT-USD', 'VARA-USD', 'FIS-USD',
>                        'FX-USD', 'KARRAT-USD', 'DIA-USD', 'ERN-USD', 'UMA-
>                        USD', 'SEAM-USD', 'SUSHI-USD', 'ANKR-USD', 'RENDER-
>                        USD', 'ETH-USD', 'AVAX-USD', 'OMNI-USD', 'DOT-USD',
>                        'INDEX-USD', 'FIDA-USD', 'BTRST-USD', 'OSMO-USD', 'RAI-
>                        USD', 'CTX-USD', 'AST-USD', 'APE-USD', 'MDT-USD',
>                        'CGLD-USD', 'BAL-USD', 'BUSD-USD', 'GHST-USD', 'GRT-
>                        USD', 'CLV-USD', 'VOXEL-USD', 'COVAL-USD', 'DIMO-USD',
>                        'INV-USD', 'LTC-USD', 'WCFG-USD', 'ALGO-USD', 'POWR-
>                        USD', 'MULTI-USD', 'PAX-USD', 'FLR-USD', 'XTZ-USD',
>                        'RPL-USD', 'WAMPL-USD', 'SAFE-USD', 'MPL-USD', 'LSETH-
>                        USD', 'CBETH-USD', 'OCEAN-USD', 'SNX-USD', 'QSP-USD',
>                        'SUKU-USD', 'HONEY-USD', 'SHPING-USD', 'ACX-USD', 'DYP-
>                        USD', 'TRB-USD', 'BLUR-USD', 'ZEC-USD', 'ENS-USD',
>                        'XRP-USD', 'WIF-USD', 'LDO-USD', 'BLZ-USD', 'ETC-USD',
>                        'SPA-USD', 'COMP-USD', 'NCT-USD', 'TIA-USD', 'EURC-
>                        USD', 'KRL-USD', 'PRIME-USD', 'BICO-USD', 'KNC-USD',
>                        'ME-USD', 'SWFTC-USD', 'UPI-USD', 'C98-USD', '00-USD',
>                        'QI-USD', 'ROSE-USD', 'CVX-USD', 'NEST-USD', 'MINA-
>                        USD', 'BADGER-USD', 'HNT-USD', 'PERP-USD', 'AERGO-USD',
>                        'QNT-USD', 'SKL-USD', 'TRU-USD', 'LQTY-USD', 'WAXL-
>                        USD', 'ZRX-USD', 'DNT-USD', 'CRPT-USD', 'MKR-USD',
>                        'IDEX-USD', 'MATIC-USD', 'GALA-USD', 'DREP-USD', 'KEEP-
>                        USD', 'BTC-USD', 'RARE-USD', 'T-USD', 'MCO2-USD',
>                        'AERO-USD', 'BIGTIME-USD', 'RARI-USD', 'ALEO-USD',
>                        'AIOZ-USD', 'LOOM-USD', '1INCH-USD', 'PUNDIX-USD',
>                        'HFT-USD', 'VELO-USD', 'FLOKI-USD', 'BIT-USD', 'KAVA-
>                        USD', 'OP-USD', 'SYN-USD', 'IMX-USD', 'ATOM-USD', 'ATA-
>                        USD'], type='csv', interval=3600, all_tokens=True,
>                        refresh_24h=False)

\*Downloads and maintains historical price data for Coinbase tokens,
saving to files.

Args: folder_path (str): Path where token data files will be stored.
Defaults to “../data/coinbase” token_list (list): List of token IDs to
process. Defaults to all USD trading pairs from coinbase_usd_tokens()
type (str): File format to save data - either “csv” or “parquet”.
Defaults to “csv” interval (int): Time interval in seconds between price
points. Defaults to 3600 (1 hour) all_tokens (bool): If True, includes
any additional tokens found in the folder path. Defaults to True
refresh_24h (bool): If True, replaces at least 24 hours. Defaults to
False

The function: - Creates the folder_path if it doesn’t exist - Date/Time
is UTC - For each token, checks if data file exists: - If exists: Loads
file and appends any new data since last recorded date - If not exists:
Downloads full history starting from 2016 - Saves data in specified
format, handling duplicates and sorting by date - For hourly data
(interval=3600), aligns to hour boundaries\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L266"
target="_blank" style="float:right; font-size:smaller">source</a>

### coinbase_data_update

>      coinbase_data_update (folder_path='../data/coinbase', token_list=['AAVE-
>                            USD'], type='parquet', interval=3600,
>                            all_tokens=False, refresh_24h=True)

\*Downloads new historical price data for Coinbase tokens… no saving

Args: folder_path (str): Path where token data files will be stored.
Defaults to “../data/coinbase” token_list (list): List of token IDs to
process. Defaults to all USD trading pairs from coinbase_usd_tokens()
type (str): File format to save data - either “csv” or “parquet”.
Defaults to “csv” interval (int): Time interval in seconds between price
points. Defaults to 3600 (1 hour) all_tokens (bool): If True, includes
any additional tokens found in the folder path. Defaults to True
refresh_24h (bool): If True, replaces at least 24 hours. Defaults to
False Returns: df (pandas.DataFrame): DataFrame of historical price data
The function: - Creates the folder_path if it doesn’t exist - Date/Time
is UTC - For each token, checks if data file exists: - If exists: Loads
file and appends any new data since last recorded date - If not exists:
Downloads full history starting from 2016 - Saves data in specified
format, handling duplicates and sorting by date - For hourly data
(interval=3600), aligns to hour boundaries\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L340"
target="_blank" style="float:right; font-size:smaller">source</a>

### read_all_files

>      read_all_files (folder_path='../data/coinbase', type='csv')

\*Read and combine all files from a folder into a single DataFrame.

Args: folder_path (str): Path to the folder containing the files.
Defaults to “../data/coinbase”. type (str): File type to read - either
“csv” or “parquet”. Defaults to “csv”.

Returns: pandas.DataFrame: Combined DataFrame containing data from all
files in the folder.

Raises: ValueError: If file type is not supported (must be “csv” or
“parquet”).\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L368"
target="_blank" style="float:right; font-size:smaller">source</a>

### coinbase_price_last_day

>      coinbase_price_last_day (pair='BTC-USD')

\*Fetch the last day’s price for a given Coinbase token. This fuction is
a helper function around coinbase_price_history().

Args: pair (str): Coinbase token pair, e.g., “BTC-USD”. Defaults to
“BTC-USD”.

Returns: pandas dataframe: Last 24 hours price data.\*

------------------------------------------------------------------------

<a
href="https://github.com/silvaac/token_style/blob/main/token_style/coinbase.py#L387"
target="_blank" style="float:right; font-size:smaller">source</a>

### binance_format

>      binance_format (df)

\*Convert coinbase dataframe to binance format.

Args: df (pandas dataframe): Coinbase dataframe.

Returns: pandas dataframe: Binance format dataframe.\*

## Examples

Load the package

``` python
from token_style.coinbase import *
```

List the available tokens on Coinbase.

``` python
tokens = coinbase_tokens()
print(tokens)
```

                id base_currency quote_currency quote_increment base_increment  \
    0     ELA-USDT           ELA           USDT           0.001           0.01   
    1     ATOM-BTC          ATOM            BTC       0.0000001            0.1   
    2     BAND-USD          BAND            USD           0.001           0.01   
    3      ACH-USD           ACH            USD        0.000001            0.1   
    4     GHST-USD          GHST            USD           0.001           0.01   
    ..         ...           ...            ...             ...            ...   
    651   XCN-USDT           XCN           USDT         0.00001            0.1   
    652    CHZ-EUR           CHZ            EUR          0.0001            0.1   
    653   ALGO-USD          ALGO            USD          0.0001            0.1   
    654  TRIBE-USD         TRIBE            USD          0.0001            0.1   
    655    COW-USD           COW            USD          0.0001            0.1   

        display_name min_market_funds  margin_enabled  post_only  limit_only  \
    0       ELA-USDT                1           False      False       False   
    1       ATOM-BTC         0.000016           False      False       False   
    2       BAND-USD                1           False      False       False   
    3        ACH-USD                1           False      False       False   
    4       GHST-USD                1           False      False       False   
    ..           ...              ...             ...        ...         ...   
    651     XCN-USDT                1           False      False       False   
    652      CHZ-EUR             0.84           False      False       False   
    653     ALGO-USD                1           False      False       False   
    654    TRIBE-USD                1           False      False       False   
    655      COW/USD                1           False      False       False   

         cancel_only    status status_message  trading_disabled  fx_stablecoin  \
    0          False  delisted                             True          False   
    1          False    online                            False          False   
    2          False    online                            False          False   
    3          False    online                            False          False   
    4          False    online                            False          False   
    ..           ...       ...            ...               ...            ...   
    651        False  delisted                             True          False   
    652        False    online                            False          False   
    653        False    online                            False          False   
    654        False  delisted                             True          False   
    655        False    online                            False          False   

        max_slippage_percentage  auction_mode high_bid_limit_percentage  
    0                0.03000000         False                            
    1                0.03000000         False                            
    2                0.03000000         False                            
    3                0.03000000         False                            
    4                0.03000000         False                            
    ..                      ...           ...                       ...  
    651              0.03000000         False                            
    652              0.03000000         False                            
    653              0.03000000         False                            
    654              0.03000000         False                            
    655              0.03000000         False                            

    [656 rows x 18 columns]

To make it simpler, we can filter the tokens by the quote currency. In
this case, we filter for USD.

``` python
usd_tokens = coinbase_usd_tokens()
print(usd_tokens)
```

                id base_currency quote_currency quote_increment base_increment  \
    2     BAND-USD          BAND            USD           0.001           0.01   
    3      ACH-USD           ACH            USD        0.000001            0.1   
    4     GHST-USD          GHST            USD           0.001           0.01   
    7     SAFE-USD          SAFE            USD          0.0001           0.01   
    8      AMP-USD           AMP            USD         0.00001              1   
    ..         ...           ...            ...             ...            ...   
    646   ORCA-USD          ORCA            USD          0.0001           0.01   
    648    DAI-USD           DAI            USD          0.0001        0.00001   
    653   ALGO-USD          ALGO            USD          0.0001            0.1   
    654  TRIBE-USD         TRIBE            USD          0.0001            0.1   
    655    COW-USD           COW            USD          0.0001            0.1   

        display_name min_market_funds  margin_enabled  post_only  limit_only  \
    2       BAND-USD                1           False      False       False   
    3        ACH-USD                1           False      False       False   
    4       GHST-USD                1           False      False       False   
    7       SAFE/USD                1           False      False       False   
    8        AMP-USD                1           False      False       False   
    ..           ...              ...             ...        ...         ...   
    646     ORCA-USD                1           False      False       False   
    648      DAI-USD                1           False      False       False   
    653     ALGO-USD                1           False      False       False   
    654    TRIBE-USD                1           False      False       False   
    655      COW/USD                1           False      False       False   

         cancel_only    status status_message  trading_disabled  fx_stablecoin  \
    2          False    online                            False          False   
    3          False    online                            False          False   
    4          False    online                            False          False   
    7          False    online                            False          False   
    8          False    online                            False          False   
    ..           ...       ...            ...               ...            ...   
    646        False    online                            False          False   
    648        False    online                            False           True   
    653        False    online                            False          False   
    654        False  delisted                             True          False   
    655        False    online                            False          False   

        max_slippage_percentage  auction_mode high_bid_limit_percentage  
    2                0.03000000         False                            
    3                0.03000000         False                            
    4                0.03000000         False                            
    7                0.03000000         False                            
    8                0.03000000         False                            
    ..                      ...           ...                       ...  
    646              0.03000000         False                            
    648              0.01000000         False                0.03000000  
    653              0.03000000         False                            
    654              0.03000000         False                            
    655              0.03000000         False                            

    [317 rows x 18 columns]

Download the historical price of BTC-USD for few months at every hour.
IMPORTANT: datetime is in UTC!

``` python
df = coinbase_price_history()
print(df)
```

                    datetime       low      high      open     close       volume  \
    0    2024-01-01 00:00:00  42261.58  42543.64  42288.58  42452.66   379.197253   
    1    2024-01-01 01:00:00  42415.00  42749.99  42453.83  42594.68   396.201924   
    2    2024-01-01 02:00:00  42488.03  42625.68  42594.58  42571.32   227.141166   
    3    2024-01-01 03:00:00  42235.00  42581.26  42571.32  42325.11   306.005694   
    4    2024-01-01 04:00:00  42200.00  42393.48  42325.10  42389.77   296.233644   
    ...                  ...       ...       ...       ...       ...          ...   
    2900 2024-04-30 20:00:00  59087.36  60082.82  59098.18  59869.99  1655.908771   
    2901 2024-04-30 21:00:00  59862.17  60326.56  59865.66  60100.36   615.103455   
    2902 2024-04-30 22:00:00  60070.00  60990.48  60100.01  60526.64   727.224395   
    2903 2024-04-30 23:00:00  60330.92  60878.39  60529.65  60622.10   346.354179   
    2904 2024-05-01 00:00:00  60015.99  60785.49  60621.20  60173.03   499.935743   

             pair  
    0     BTC-USD  
    1     BTC-USD  
    2     BTC-USD  
    3     BTC-USD  
    4     BTC-USD  
    ...       ...  
    2900  BTC-USD  
    2901  BTC-USD  
    2902  BTC-USD  
    2903  BTC-USD  
    2904  BTC-USD  

    [2905 rows x 7 columns]

Download all history available in coinbase for the tokens saved in
folder “../data/coinbase” This process can take a while specially if you
are downloading many tokens the first time.

Now read the file from folder “../data/coinbase”:

``` python
import pandas as pd
df = pd.read_parquet("../data/coinbase/AAVE-USD.parquet")
print(df)
```

                     datetime      low     high     open    close    volume  \
    0     2020-12-15 17:00:00   87.700   90.400   90.400   88.347  4289.548   
    1     2020-12-15 18:00:00   85.620   88.407   88.010   86.000  3802.529   
    2     2020-12-15 19:00:00   84.575   89.500   86.105   85.952  2835.124   
    3     2020-12-15 20:00:00   84.828   85.952   85.952   85.599   798.670   
    4     2020-12-15 21:00:00   84.744   85.893   85.599   85.208   442.464   
    ...                   ...      ...      ...      ...      ...       ...   
    35608 2025-01-07 14:00:00  323.950  333.200  332.900  326.300  2494.366   
    35609 2025-01-07 15:00:00  310.900  327.070  326.300  315.770  9817.720   
    35610 2025-01-07 16:00:00  313.530  318.250  315.820  314.720  1782.992   
    35611 2025-01-07 17:00:00  310.610  315.620  314.780  312.660  3426.675   
    35612 2025-01-07 18:00:00  311.970  313.360  312.640  312.950   255.633   

               pair  
    0      AAVE-USD  
    1      AAVE-USD  
    2      AAVE-USD  
    3      AAVE-USD  
    4      AAVE-USD  
    ...         ...  
    35608  AAVE-USD  
    35609  AAVE-USD  
    35610  AAVE-USD  
    35611  AAVE-USD  
    35612  AAVE-USD  

    [35613 rows x 7 columns]

Full download with no files in the folder takes almost 5 hours. After
that, it is much faster (~ 6 minutes). Clearly it will depend on your
internet speed and the last time you did the download.

``` python
coinbase_to_file(type="parquet") # takes almost 5 hours if there no files in the folder!
```

Read all files in the folder into a single dataframe:

``` python
df = read_all_files(type="parquet")
print(df)
```

                     datetime       low      high      open     close  \
    0     2022-10-11 17:00:00  0.372500  0.510000  0.450000  0.510000   
    1     2022-10-11 18:00:00  0.427800  1.284200  0.507800  1.284200   
    2     2022-10-11 19:00:00  0.900900  1.464600  1.269000  1.300000   
    3     2022-10-11 20:00:00  1.140000  1.382000  1.292100  1.331900   
    4     2022-10-11 21:00:00  1.268600  1.649800  1.331900  1.546400   
    ...                   ...       ...       ...       ...       ...   
    54654 2025-01-06 15:00:00  0.547015  0.556522  0.548198  0.554798   
    54655 2025-01-06 16:00:00  0.547348  0.557769  0.553998  0.551552   
    54656 2025-01-06 17:00:00  0.540932  0.551227  0.550957  0.546732   
    54657 2025-01-06 18:00:00  0.543649  0.549062  0.546999  0.546143   
    54658 2025-01-06 19:00:00  0.545549  0.550554  0.546045  0.548921   

                 volume     pair  
    0      2.214310e+05   00-USD  
    1      1.261266e+06   00-USD  
    2      1.587872e+06   00-USD  
    3      6.753579e+05   00-USD  
    4      8.359198e+05   00-USD  
    ...             ...      ...  
    54654  3.270357e+05  ZRX-USD  
    54655  2.239506e+05  ZRX-USD  
    54656  2.097155e+05  ZRX-USD  
    54657  1.189937e+05  ZRX-USD  
    54658  5.694513e+04  ZRX-USD  

    [6827588 rows x 7 columns]

Download the last day’s price for AAVE-USD:

``` python
# eval: false
coinbase_price_last_day(pair='AAVE-USD')
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
&#10;    .dataframe tbody tr th {
        vertical-align: top;
    }
&#10;    .dataframe thead th {
        text-align: right;
    }
</style>

<table class="dataframe" data-quarto-postprocess="true" data-border="1">
<thead>
<tr class="header" style="text-align: right;">
<th data-quarto-table-cell-role="th"></th>
<th data-quarto-table-cell-role="th">datetime</th>
<th data-quarto-table-cell-role="th">low</th>
<th data-quarto-table-cell-role="th">high</th>
<th data-quarto-table-cell-role="th">open</th>
<th data-quarto-table-cell-role="th">close</th>
<th data-quarto-table-cell-role="th">volume</th>
<th data-quarto-table-cell-role="th">pair</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td data-quarto-table-cell-role="th">23</td>
<td>2025-01-06 19:00:00</td>
<td>343.79</td>
<td>347.51</td>
<td>344.22</td>
<td>346.08</td>
<td>696.867</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">22</td>
<td>2025-01-06 20:00:00</td>
<td>342.17</td>
<td>346.63</td>
<td>346.19</td>
<td>343.71</td>
<td>1024.476</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">21</td>
<td>2025-01-06 21:00:00</td>
<td>340.36</td>
<td>344.01</td>
<td>343.62</td>
<td>340.47</td>
<td>1223.404</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">20</td>
<td>2025-01-06 22:00:00</td>
<td>339.69</td>
<td>342.42</td>
<td>340.51</td>
<td>340.01</td>
<td>1577.239</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">19</td>
<td>2025-01-06 23:00:00</td>
<td>337.55</td>
<td>341.68</td>
<td>340.01</td>
<td>341.65</td>
<td>1224.641</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">18</td>
<td>2025-01-07 00:00:00</td>
<td>338.43</td>
<td>342.70</td>
<td>341.52</td>
<td>338.54</td>
<td>917.205</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">17</td>
<td>2025-01-07 01:00:00</td>
<td>337.49</td>
<td>343.00</td>
<td>338.56</td>
<td>342.10</td>
<td>1536.442</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">16</td>
<td>2025-01-07 02:00:00</td>
<td>337.82</td>
<td>343.34</td>
<td>342.28</td>
<td>338.23</td>
<td>2047.035</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">15</td>
<td>2025-01-07 03:00:00</td>
<td>336.86</td>
<td>339.90</td>
<td>338.42</td>
<td>339.52</td>
<td>1057.940</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">14</td>
<td>2025-01-07 04:00:00</td>
<td>337.47</td>
<td>340.03</td>
<td>339.48</td>
<td>337.54</td>
<td>860.860</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">13</td>
<td>2025-01-07 05:00:00</td>
<td>337.11</td>
<td>338.98</td>
<td>337.60</td>
<td>338.91</td>
<td>1117.530</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">12</td>
<td>2025-01-07 06:00:00</td>
<td>337.59</td>
<td>339.88</td>
<td>338.86</td>
<td>339.29</td>
<td>759.073</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">11</td>
<td>2025-01-07 07:00:00</td>
<td>337.02</td>
<td>340.44</td>
<td>339.31</td>
<td>337.63</td>
<td>309.481</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">10</td>
<td>2025-01-07 08:00:00</td>
<td>336.80</td>
<td>339.58</td>
<td>337.62</td>
<td>337.44</td>
<td>697.592</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">9</td>
<td>2025-01-07 09:00:00</td>
<td>333.31</td>
<td>337.62</td>
<td>337.50</td>
<td>335.13</td>
<td>1350.081</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">8</td>
<td>2025-01-07 10:00:00</td>
<td>332.76</td>
<td>335.13</td>
<td>335.13</td>
<td>334.46</td>
<td>462.950</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">7</td>
<td>2025-01-07 11:00:00</td>
<td>330.50</td>
<td>335.83</td>
<td>334.43</td>
<td>333.38</td>
<td>1858.256</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">6</td>
<td>2025-01-07 12:00:00</td>
<td>330.64</td>
<td>333.90</td>
<td>333.42</td>
<td>332.69</td>
<td>909.802</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">5</td>
<td>2025-01-07 13:00:00</td>
<td>331.88</td>
<td>334.83</td>
<td>332.73</td>
<td>332.79</td>
<td>706.971</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">4</td>
<td>2025-01-07 14:00:00</td>
<td>323.95</td>
<td>333.20</td>
<td>332.90</td>
<td>326.30</td>
<td>2494.366</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">3</td>
<td>2025-01-07 15:00:00</td>
<td>310.90</td>
<td>327.07</td>
<td>326.30</td>
<td>315.77</td>
<td>9817.720</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">2</td>
<td>2025-01-07 16:00:00</td>
<td>313.53</td>
<td>318.25</td>
<td>315.82</td>
<td>314.72</td>
<td>1782.992</td>
<td>AAVE-USD</td>
</tr>
<tr class="odd">
<td data-quarto-table-cell-role="th">1</td>
<td>2025-01-07 17:00:00</td>
<td>310.61</td>
<td>315.62</td>
<td>314.78</td>
<td>312.66</td>
<td>3426.675</td>
<td>AAVE-USD</td>
</tr>
<tr class="even">
<td data-quarto-table-cell-role="th">0</td>
<td>2025-01-07 18:00:00</td>
<td>308.59</td>
<td>314.32</td>
<td>312.64</td>
<td>309.27</td>
<td>1807.796</td>
<td>AAVE-USD</td>
</tr>
</tbody>
</table>

</div>

Update the data for AAVE-USD and convert it to Binance format (minor
changes in column names and sequence):

``` python
df = coinbase_data_update(token_list=['BTC-USD'])
binance_df = binance_format(df)
print(binance_df)
```

    Processing BTC-USD
                               date       Open       High       Low     Close  \
    0     2016-01-01 00:00:00+00:00     430.35     431.82    430.35    430.61   
    1     2016-01-01 01:00:00+00:00     430.59     430.98    430.00    430.78   
    2     2016-01-01 02:00:00+00:00     430.80     430.89    430.50    430.62   
    3     2016-01-01 03:00:00+00:00     430.62     432.84    430.31    432.84   
    4     2016-01-01 04:00:00+00:00     432.74     437.15    432.72    436.12   
    ...                         ...        ...        ...       ...       ...   
    79024 2025-01-07 14:00:00+00:00  100635.66  100874.75  99714.26  99966.01   
    79025 2025-01-07 15:00:00+00:00   99963.14  100120.80  97100.00  97902.38   
    79026 2025-01-07 16:00:00+00:00   97906.68   98324.08  97500.01  97705.66   
    79027 2025-01-07 17:00:00+00:00   97705.66   97779.40  96545.81  97233.28   
    79028 2025-01-07 18:00:00+00:00   97233.25   97519.49  96545.00  96633.74   

                Volume pair  
    0       160.179593  btc  
    1        92.132315  btc  
    2        93.024327  btc  
    3       103.811532  btc  
    4       363.875951  btc  
    ...            ...  ...  
    79024  1006.819765  btc  
    79025  3947.701262  btc  
    79026  1181.333506  btc  
    79027   977.511431  btc  
    79028   687.292645  btc  

    [79029 rows x 7 columns]