Stock Price Prediction Analytics using Snowflake and Airflow
Project Overview

This project demonstrates how a stock price forecasting analytics system was designed and implemented using Snowflake and Apache Airflow. Historical stock data for IBM (IBM), Cisco (CSCO), Microsoft (MSFT), Apple (AAPL), NVIDIA (NVDA), and Amazon (AMZN) are retrieved using the yfinance API, processed through Airflow pipelines, and stored in Snowflake. A secondary machine learning (ML) forecasting pipeline is used to predict future stock prices. Finally, both historical and forecast data are combined into a single unified table in Snowflake for analytics and visualization.

Technologies Used

Python: Data extraction, processing, and orchestration

Snowflake: Cloud-based data warehouse for storage and computation

Apache Airflow: Workflow automation and task scheduling

yfinance API: Data source for historical stock prices

SQL: Data manipulation, transactions, and table operations

Snowflake ML Forecast: In-database time-series forecasting for price prediction

Project Workflow

Data Extraction

Historical stock price data for six companies (IBM, Cisco, Microsoft, Apple, NVIDIA, Amazon) is fetched from the yfinance API for the last 180 days.

The list of symbols and lookback window are defined in Airflow Variables (SYMBOLS_JSON, LOOKBACK_DAYS).

Data Storage

Extracted data is loaded into Snowflake tables under the following structure:

RAW.MARKET_DATA: Stores historical stock prices.

RAW.MARKET_DATA_STG: Used for transactional staging and merging.

Data Processing & ML Forecasting

The Snowflake ML Forecast model is trained on the historical data from RAW.MARKET_DATA.

The model generates 7-day stock price forecasts for each symbol.

Forecasted results are stored in:

ADHOC.MARKET_DATA_FORECAST

Final combined analytics table: ANALYTICS.MARKET_DATA (actuals ∪ forecasts)

Automated Data Pipeline (Managed by Airflow DAGs)

DAG 1 – part1_create_and_load:
Creates required schemas and tables, fetches yfinance data, and loads it into Snowflake using a transactional MERGE operation.

DAG 2 – part2_train_predict:
Creates a view for training, trains the forecasting model, generates predictions, and unions results into a final analytics table.

Both DAGs use Airflow Connections (snowflake_conn) and Variables for configuration and are fully automated.

Installation & Setup
Requirements

Python 3.8 or higher

Snowflake account with access credentials

Apache Airflow (Cloud Composer or Local Setup)

Required packages:

apache-airflow-providers-snowflake

yfinance

pandas

Airflow Configuration

In Airflow:

Connection ID: snowflake_conn

Add Snowflake account details (user, password, account, warehouse, database).

Variables:

SYMBOLS_JSON = ["IBM", "CSCO", "MSFT", "AAPL", "NVDA", "AMZN"]

LOOKBACK_DAYS = 180

HISTORICAL_TABLE = "RAW.MARKET_DATA"

Snowflake Setup

Create the required database and schemas in Snowflake:

Database: FINANCE

Schemas: RAW, ADHOC, ANALYTICS

Run the following DAGs in Airflow:

part1_create_and_load (Data extraction and loading)

part2_train_predict (Model training, forecasting, and final union)

Results & Findings

The system automatically retrieves and updates 180 days of data for all configured tickers.

Forecasts for the next 7 business days are generated using Snowflake ML Forecast, achieving reliable accuracy for short-term trends.

Airflow DAGs ensure automation, error handling, and transactional consistency.

Snowflake provides scalable storage, fast SQL queries, and in-database ML capabilities.

Future Enhancements

Incorporate deep learning models (e.g., LSTM) for improved prediction accuracy.

Enable real-time stock streaming using APIs such as WebSocket or Kafka.

Enhance the feature set with additional technical indicators.

Integrate visualization tools such as Tableau or Power BI for business insights.

Add alerting mechanisms for significant stock trend deviations.

License

This project is licensed under the MIT License.
