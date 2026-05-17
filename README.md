# **`README.md`**

# The Value of Information: An Empirical Replication Pipeline

<!-- PROJECT SHIELDS -->
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![arXiv](https://img.shields.io/badge/arXiv-2605.11180-b31b1b.svg)](https://arxiv.org/abs/2605.11180)
[![Journal](https://img.shields.io/badge/Journal-ArXiv%20Preprint-003366)](https://arxiv.org/abs/2605.11180)
[![Year](https://img.shields.io/badge/Year-2026-purple)](https://github.com/chirindaopensource/the_value_of_information)
[![Discipline: Microstructure](https://img.shields.io/badge/Discipline-Market%20Microstructure-00529B)](https://github.com/chirindaopensource/the_value_of_information)
[![Discipline: Econometrics](https://img.shields.io/badge/Discipline-High--Frequency%20Econometrics-00529B)](https://github.com/chirindaopensource/the_value_of_information)
[![Discipline: Math Finance](https://img.shields.io/badge/Discipline-Mathematical%20Finance-00529B)](https://github.com/chirindaopensource/the_value_of_information)
[![Data: NYSE TAQ](https://img.shields.io/badge/Data-NYSE%20TAQ-lightgrey)](https://wrds-www.wharton.upenn.edu/)
[![Data: CRSP](https://img.shields.io/badge/Data-CRSP-lightgrey)](https://wrds-www.wharton.upenn.edu/)
[![Data: Compustat](https://img.shields.io/badge/Data-Compustat-lightgrey)](https://wrds-www.wharton.upenn.edu/)
[![Data: CPI](https://img.shields.io/badge/Data-BLS%20CPI-lightgrey)](https://www.bls.gov/cpi/)
[![Data: ICI](https://img.shields.io/badge/Data-ICI%20Fact%20Book-lightgrey)](https://www.icifactbook.org/)
[![Method: Covariation](https://img.shields.io/badge/Method-Quadratic%20Covariation-orange)](https://github.com/chirindaopensource/the_value_of_information)
[![Method: CLNV](https://img.shields.io/badge/Method-CLNV%20Trade%20Signing-orange)](https://github.com/chirindaopensource/the_value_of_information)
[![Method: SDF Bounds](https://img.shields.io/badge/Method-SDF%20Entropy%20Bounding-orange)](https://github.com/chirindaopensource/the_value_of_information)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![Type Checking: mypy](https://img.shields.io/badge/type%20checking-mypy-blue)](http://mypy-lang.org/)
[![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=flat&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Statsmodels](https://img.shields.io/badge/statsmodels-%230C55A5.svg?style=flat)](https://www.statsmodels.org/)
[![Linearmodels](https://img.shields.io/badge/linearmodels-%23CB171E.svg?style=flat)](https://bashtage.github.io/linearmodels/)
[![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=flat&logo=scipy&logoColor=white)](https://scipy.org/)
[![YAML](https://img.shields.io/badge/YAML-%23CB171E.svg?style=flat&logo=yaml&logoColor=white)](https://yaml.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-%23F37626.svg?style=flat&logo=Jupyter&logoColor=white)](https://jupyter.org/)
[![Open Source](https://img.shields.io/badge/Open%20Source-%E2%9D%A4-brightgreen)](https://github.com/chirindaopensource/framework_for_high_volume_procurement_oversight_and_decision_support)

**Repository:** `https://github.com/chirindaopensource/the_value_of_information`

**Owner:** 2026 Craig Chirinda (Open Source Projects)

## Introduction

This project is an independent, professional-grade implementation of the ideas and econometric methodologies from the paper which is titled **"The Value of Information: A Puzzle"** by the authors:
*   **Ohad Kadan**
*   **Asaf Manela**

This repository provides a complete, end-to-end computational framework for replicating the paper's findings. It delivers a highly optimized, memory-efficient pipeline that processes massive, unstructured high-frequency tick data (NYSE TAQ) to estimate the equilibrium dollar value of private information in US equity markets. By transitioning from a theoretical identity (noise trader losses) to an estimable econometric moment (the quadratic covariation between price changes and order flow), this codebase establishes a robust "Information Barometer" for financial engineering.

## Table of Contents

- [Introduction](#introduction)
- [Theoretical Background](#theoretical-background)
- [Features](#features)
- [Methodology Implemented](#methodology-implemented)
- [Core Components (Notebook Structure)](#core-components-notebook-structure)
- [Key Callable: execute_master_replication_pipeline](#key-callable-execute_master_replication_pipeline)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Input Data Structure](#input-data-structure)
- [Usage](#usage)
- [Output Structure](#output-structure)
- [Project Structure](#project-structure)
- [Customization](#customization)
- [Contributing](#contributing)
- [Recommended Extensions](#recommended-extensions)
- [License](#license)
- [Citation](#citation)
- [Acknowledgments](#acknowledgments)

## Theoretical Background

The implemented methods bridge continuous-time stochastic calculus with high-frequency discrete data engineering.

**1. The Kyle-Back Equilibrium and Orthogonality:**
In a market with competitive market makers, informed trader gains ($\Omega$) exactly equal noise trader losses. Because strategic informed traders "shade" their information to minimize price impact, their cumulative order process ($X_t$) is locally deterministic ("smooth"). Consequently, the quadratic covariation between informed trades and price changes vanishes. This allows us to substitute unobservable noise trading ($dZ$) with observable total order flow ($dY$):
$$ \Omega = E \int_0^T dP_t dY_t $$

**2. The Discrete Quadratic Covariation Estimator:**
To operationalize this on TAQ data, the continuous integral is approximated over equidistant intervals (e.g., $h=1$ minute) and annualized:
$$ \hat{\Omega}_{jt} = \frac{1}{T} \sum_{i=1}^N \Delta P_{jti} \Delta Y_{jti} $$

**3. Log-Linear Decomposition:**
The value of information is decomposed into price impact ($\lambda$) and order flow variance ($\sigma^2_y$) to isolate liquidity frictions from trading intensity:
$$ \log(\Omega/C) = \log \tilde{\lambda} + \log \tilde{\sigma}^2_y $$

**4. SDF Entropy Bounding (Risk Adjustment):**
To test if risk aversion resolves the puzzle (where fees of 0.67% dwarf the 0.04% value of information), the pipeline computes the theoretical upper bound for risk-adjusted value ($\Omega_M$) using the Alvarez-Jermann (2005) entropy framework:
$$ \Omega_M \le \Omega + \sigma(\Omega) \sqrt{e^{2L(M)} - 1} $$

Below is a diagram which summarizes the proposed approach:

<div align="center">
  <img src="https://github.com/chirindaopensource/the_value_of_information/blob/main/the_value_of_information_ipo_main.png" alt="Information Barometer Architecture" width="100%">
</div>


## Features

-   **Strict Temporal Monotonicity:** Enforces stable sorting (`kind='mergesort'`) to preserve the deterministic sequence of nanosecond-resolution ticks, preventing arbitrary shuffling of simultaneous events.
-   **Sparse Grid Architecture:** Avoids catastrophic Cartesian cross-joins by mapping asynchronous trades to a synchronous mathematical grid using bounded `merge_asof` operations, mathematically eliminating cross-day temporal leakage.
-   **Volume-Weighted Average (VWA) Quote Aggregation:** Aggregates simultaneous quote updates to preserve all limit order book liquidity information, ensuring highly accurate midpoints for the CLNV signing algorithm.
-   **Explicit Memory Management:** Utilizes aggressive garbage collection (`del` and `gc.collect()`) to prevent Out-Of-Memory (OOM) crashes when processing decades of TAQ data.
-   **Exact Finite-Sample Inference:** Extracts exact confidence intervals directly from the `linearmodels` econometric engine for multi-way clustered panel data, avoiding statistically invalid manual asymptotic approximations.
-   **Cryptographic Audit Ledgers:** Generates SHA-256 hashed replication manifests to guarantee the provenance and immutability of the empirical results.

## Methodology Implemented

1.  **Data Ingestion & Validation (Tasks 1-3):** Validates schemas, enforces timezone awareness, and audits pre-cleaning microstructure anomalies.
2.  **TAQ Cleansing & Synchronization (Tasks 4-6):** Filters irregular TAQ conditions, enforces $P>0$, aggregates simultaneous quotes via VWA, and executes a backward as-of merge to attach prevailing quotes to trades.
3.  **Trade Signing & Discretization (Tasks 7-9):** Applies the Chakrabarty et al. (2007) algorithm to sign volume, expands the data onto a 1-minute sparse grid, and computes the discrete first differences ($\Delta P$, $\Delta Y$).
4.  **Estimation & Decomposition (Tasks 10-12):** Computes the core $\hat{\Omega}_{jt}$ estimator, estimates price impact via no-intercept OLS, and constructs the log-linear decomposition.
5.  **Sample Selection & Aggregation (Tasks 13-15):** Applies the Amihud (2002) $5 price floor, executes strictly annual 1% winsorization (preventing look-ahead bias), and computes the daily market-cap share.
6.  **Event Studies & Regressions (Tasks 16-18):** Identifies true earnings dates via the Engelberg et al. (2018) volume heuristic, executes the 45-day event study, and runs high-dimensional `PanelOLS` regressions with two-way clustering.
7.  **Risk Bounding & Certification (Tasks 25-26):** Computes the SDF entropy bounds to evaluate the "Puzzle" and generates the final, objective replication certification report.

## Core Components (Notebook Structure)

*Note: All 27 orchestrator callables and their constituent helper functions are contained within a singular, comprehensive Jupyter Notebook.*

The notebook is structured as a logical Directed Acyclic Graph (DAG), moving from raw data ingestion through microstructure engineering, econometric estimation, and final theoretical bounding.

## Key Callable: `execute_master_replication_pipeline`

The project is designed around a single, top-level user-facing interface function:

-   **`execute_master_replication_pipeline`:** This apex orchestrator function runs the entire automated research pipeline from end-to-end. A single call to this function reproduces the entire computational portion of the project, managing memory explicitly across the Baseline run, the Robustness Suites (Signing, Frequency, Regression), the Risk-Adjustment Bounding, and the Final Certification. It returns a comprehensive dictionary containing the `BaselineReportingBundle` and the cryptographic `PipelineAuditLedger`.

## Prerequisites

-   Python 3.10+
-   Core Python dependencies: `numpy`, `pandas`, `scipy`, `statsmodels`, `linearmodels`, `pyyaml`, `Faker`.

## Installation

1.  **Clone the repository:**
    ```sh
    git clone https://github.com/chirindaopensource/the_value_of_information.git
    cd the_value_of_information
    ```

2.  **Create and activate a virtual environment (recommended):**
    ```sh
    python -m venv venv
    source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
    ```

3.  **Install Python dependencies:**
    ```sh
    pip install numpy pandas scipy statsmodels linearmodels pyyaml Faker
    ```

## Input Data Structure

The pipeline requires 8 strictly formatted `pandas.DataFrame` structures:
1.  **`df_raw_trades`**: The physical tier of transaction events (`ticker`, `ts_val`, `price`, `size`, `sale_condition`, etc.).
2.  **`df_raw_quotes`**: The limit order book context (`ticker`, `ts_val`, `bid`, `ask`, `quote_condition`, etc.).
3.  **`df_raw_meta`**: The stock-day reference panel containing CRSP/Compustat fundamentals (`market_equity`, `book_equity`, `earnings_ann_date`).
4.  **`df_id_link`**: The TAQ-to-CRSP linkage table (`ticker`, `permno`, `link_start_date`, `link_end_date`).
5.  **`df_market_calendar`**: The session boundary definitions (`session_open`, `session_close`).
6.  **`df_cpi`**: The inflation scaling factors.
7.  **`df_agg_mktcap`**: The prior-month aggregate market cap denominator.
8.  **`df_crsp_monthly`**: Auxiliary monthly returns for momentum construction.

## Usage

Here is the granular, step-by-step guide to executing the end-to-end pipeline for **"The Value of Information: A Puzzle"**. This example demonstrates how to synthetically generate the required high-frequency TAQ and low-frequency CRSP/Compustat data structures, load the study configuration from a YAML file, and execute the full research pipeline using the `execute_master_replication_pipeline` orchestrator.

*Note: As per the architectural constraints of this environment, we assume that all the callables defined previously in this conversation are already loaded into memory within a single Jupyter Notebook. There is no reliance on external `.py` module imports for the pipeline functions.*

```python
# Import the pandas library for high-performance data manipulation.
import pandas as pd
# Import the numpy library for vectorized numerical operations and random sampling.
import numpy as np
# Import the os module for operating system interactions (e.g., file path checks).
import os
# Import the yaml module to parse the external configuration file.
import yaml
# Import the Faker library to generate realistic synthetic identifiers.
from faker import Faker
# Import typing modules to enforce strict static type hints.
from typing import Dict, Any
# Import zoneinfo for IANA timezone support.
import zoneinfo
# Import timedelta for temporal offsets.
from datetime import timedelta

# Initialize the Faker instance to generate synthetic strings.
fake = Faker()
# Seed the Faker instance to guarantee deterministic output across runs.
Faker.seed(42)
# Seed the numpy random number generator to guarantee deterministic numerical simulation.
np.random.seed(42)

def generate_synthetic_taq_and_crsp_data(
    start_date: str = "2024-12-02",
    end_date: str = "2024-12-06",
    tickers: list = ["AAPL", "MSFT"]
) -> Dict[str, pd.DataFrame]:
    """
    Generates high-fidelity synthetic TAQ and CRSP DataFrames for pipeline testing.

    Purpose:
        To create a mathematically plausible, schema-compliant dataset that mimics
        the physical tier of the US equity market and its associated institutional
        metadata. This allows for the execution of the discrete quadratic covariation
        estimator (Equation 12) without requiring proprietary WRDS TAQ access.

    Inputs:
        start_date (str): The start date of the simulation in 'YYYY-MM-DD' format.
        end_date (str): The end date of the simulation in 'YYYY-MM-DD' format.
        tickers (list): A list of stock ticker symbols to simulate.

    Processes:
        1. Calendar Generation: Creates trading session boundaries localized to NY time.
        2. Tick Simulation: Generates random walk prices and corresponding NBBO quotes.
        3. Metadata Construction: Generates CRSP/Compustat fundamentals (Size, B/M).
        4. Schema Enforcement: Casts all columns to the strict types required by Task 2.

    Outputs:
        Dict[str, pd.DataFrame]: A dictionary containing the 8 required DataFrames:
            - df_raw_trades, df_raw_quotes, df_raw_meta, df_id_link,
              df_market_calendar, df_cpi, df_agg_mktcap, df_crsp_monthly.

    Raises:
        ValueError: If start_date is not strictly before or equal to end_date.
    """
    # Parse the start date into a pandas Timestamp.
    start_ts = pd.Timestamp(start_date)
    # Parse the end date into a pandas Timestamp.
    end_ts = pd.Timestamp(end_date)
    
    # Validate that the start date does not occur after the end date.
    if start_ts > end_ts:
        # Raise a ValueError to prevent invalid temporal generation.
        raise ValueError("start_date must be before or equal to end_date.")

    # Define the canonical session timezone required by the pipeline.
    tz = "America/New_York"
    # Generate a sequence of business days to represent the trading calendar.
    b_days = pd.date_range(start=start_date, end=end_date, freq='B')
    
    # ==========================================================================
    # 1. df_market_calendar
    # ==========================================================================
    # Initialize an empty list to store calendar records.
    cal_records = []
    # Iterate over each generated business day.
    for d in b_days:
        # Define the standard session open time (09:30:00).
        open_time = pd.Timestamp(f"{d.strftime('%Y-%m-%d')} 09:30:00").tz_localize(tz)
        # Define the standard session close time (16:00:00).
        close_time = pd.Timestamp(f"{d.strftime('%Y-%m-%d')} 16:00:00").tz_localize(tz)
        # Append the calendar record dictionary to the list.
        cal_records.append({
            "date": d.date(),
            "session_open": open_time,
            "session_close": close_time,
            "is_early_close": False,
            "expected_minutes": 390
        })
    # Construct the market calendar DataFrame.
    df_market_calendar = pd.DataFrame(cal_records)

    # ==========================================================================
    # 2 & 3. df_raw_trades and df_raw_quotes
    # ==========================================================================
    # Initialize empty lists to store trade and quote records.
    trade_records = []
    quote_records = []
    # Initialize a counter to serve as a deterministic sequence ID.
    seq_id = 1
    
    # Iterate over each ticker requested.
    for ticker in tickers:
        # Set an initial starting price for the random walk.
        current_price = 150.0
        # Iterate over each business day.
        for d in b_days:
            # Define the open time for the current day.
            open_time = pd.Timestamp(f"{d.strftime('%Y-%m-%d')} 09:30:00").tz_localize(tz)
            # Simulate 100 random trade timestamps within the first hour for brevity.
            # In a real scenario, this would be millions of ticks.
            time_offsets = np.sort(np.random.uniform(0, 3600, 100))
            
            # Iterate over each simulated time offset.
            for offset in time_offsets:
                # Calculate the exact nanosecond timestamp for the event.
                ts_val = open_time + pd.Timedelta(seconds=offset)
                # Simulate a price step using a normal distribution (random walk).
                current_price += np.random.normal(0, 0.05)
                # Simulate a trade size in multiples of 100 shares.
                size = int(np.random.choice([100, 200, 500, 1000]))
                
                # Append the trade record to the list.
                trade_records.append({
                    "ticker": ticker,
                    "ts_val": ts_val,
                    "trade_date": d.date(),
                    "price": round(current_price, 2),
                    "size": size,
                    "exch": "N",
                    "sale_condition": "REG",
                    "corr": 0,
                    "trade_id": str(seq_id)
                })
                
                # Simulate a prevailing quote slightly before the trade (1 millisecond prior).
                quote_ts = ts_val - pd.Timedelta(milliseconds=1)
                # Define the bid slightly below the current price.
                bid = round(current_price - 0.02, 2)
                # Define the ask slightly above the current price.
                ask = round(current_price + 0.02, 2)
                
                # Append the quote record to the list.
                quote_records.append({
                    "ticker": ticker,
                    "ts_val": quote_ts,
                    "quote_date": d.date(),
                    "bid": bid,
                    "ask": ask,
                    "bid_size": size * 2,
                    "ask_size": size * 2,
                    "exch": "N",
                    "quote_condition": "REG",
                    "is_nbbo": True,
                    "quote_id": str(seq_id)
                })
                # Increment the sequence ID to maintain deterministic ordering.
                seq_id += 1

    # Construct the raw trades DataFrame.
    df_raw_trades = pd.DataFrame(trade_records)
    # Construct the raw quotes DataFrame.
    df_raw_quotes = pd.DataFrame(quote_records)

    # ==========================================================================
    # 4. df_raw_meta
    # ==========================================================================
    # Initialize an empty list to store metadata records.
    meta_records = []
    # Map tickers to synthetic CRSP permnos.
    permno_map = {"AAPL": 10104, "MSFT": 10107}
    
    # Iterate over each ticker.
    for ticker in tickers:
        # Iterate over each business day.
        for d in b_days:
            # Append the stock-day metadata record.
            meta_records.append({
                "permno": permno_map[ticker],
                "ticker": ticker,
                "date": d.date(),
                "prev_close": 150.0, # Ensure > $5 to pass Amihud filter
                "market_equity": 2e12,
                "ln_me": np.log(2e12),
                "book_to_market": 0.3,
                "momentum_2_12": 0.15,
                "earnings_ann_date": d.date() if d == b_days[0] else pd.NaT,
                "earnings_event_date": d.date() if d == b_days[0] else pd.NaT,
                "is_earnings_day": 1 if d == b_days[0] else 0,
                "firm_volume_shares": 50000000.0,
                "market_volume_shares": 5000000000.0,
                "cpi_multiplier": 1.0
            })
    # Construct the raw metadata DataFrame.
    df_raw_meta = pd.DataFrame(meta_records)
    # Ensure date columns are properly cast to datetime objects.
    df_raw_meta['date'] = pd.to_datetime(df_raw_meta['date'])
    df_raw_meta['earnings_ann_date'] = pd.to_datetime(df_raw_meta['earnings_ann_date'])
    df_raw_meta['earnings_event_date'] = pd.to_datetime(df_raw_meta['earnings_event_date'])

    # ==========================================================================
    # 5. df_id_link
    # ==========================================================================
    # Initialize an empty list to store linkage records.
    link_records = []
    # Iterate over each ticker to create a linkage span.
    for ticker in tickers:
        # Append the linkage record covering the entire sample period.
        link_records.append({
            "ticker": ticker,
            "permno": permno_map[ticker],
            "gvkey": str(permno_map[ticker] + 1000),
            "link_start_date": pd.Timestamp("2000-01-01").date(),
            "link_end_date": pd.Timestamp("2099-12-31").date(),
            "link_confidence": "1"
        })
    # Construct the ID link DataFrame.
    df_id_link = pd.DataFrame(link_records)

    # ==========================================================================
    # 6. df_cpi
    # ==========================================================================
    # Construct a simple CPI DataFrame for the current month.
    df_cpi = pd.DataFrame({
        "date": [pd.Timestamp("2024-12-01").date()],
        "cpi_index": [310.5],
        "is_sa": [False]
    })

    # ==========================================================================
    # 7. df_agg_mktcap
    # ==========================================================================
    # Construct the aggregate market cap DataFrame for the prior month (November).
    # This is required for the denominator in Figure 4.
    df_agg_mktcap = pd.DataFrame({
        "month": [pd.Timestamp("2024-11-01").date()],
        "agg_market_cap": [50e12] # 50 Trillion USD
    })

    # ==========================================================================
    # 8. df_crsp_monthly
    # ==========================================================================
    # Construct a mock monthly CRSP DataFrame.
    crsp_records = []
    # Iterate over each ticker.
    for ticker in tickers:
        # Append the monthly return record.
        crsp_records.append({
            "permno": permno_map[ticker],
            "month": pd.Timestamp("2024-11-30").date(),
            "ret": 0.05,
            "dlret": 0.0,
            "me": 2e12
        })
    # Construct the monthly CRSP DataFrame.
    df_crsp_monthly = pd.DataFrame(crsp_records)

    # Compile all DataFrames into the final dictionary payload.
    raw_dataframes = {
        "df_raw_trades": df_raw_trades,
        "df_raw_quotes": df_raw_quotes,
        "df_raw_meta": df_raw_meta,
        "df_id_link": df_id_link,
        "df_market_calendar": df_market_calendar,
        "df_cpi": df_cpi,
        "df_agg_mktcap": df_agg_mktcap,
        "df_crsp_monthly": df_crsp_monthly
    }

    # Return the dictionary of synthetic DataFrames.
    return raw_dataframes

# Execute the generation function to load the data into memory.
raw_dataframes = generate_synthetic_taq_and_crsp_data()

# Print a diagnostic preview to verify schema compliance.
print("Synthetic TAQ Trades Preview:")
print(raw_dataframes["df_raw_trades"].head(3))

def load_study_configuration(filepath: str = "config.yaml") -> Dict[str, Any]:
    """
    Loads the study configuration parameters from a YAML file into a Python dictionary.

    Purpose:
        To ingest the deterministic hyperparameters, data cleaning rules, and
        economic benchmarks defined in the external configuration file. This ensures
        cryptographic reproducibility by separating code from configuration.

    Inputs:
        filepath (str): The relative or absolute path to the YAML configuration file.
                        Default is "config.yaml".

    Processes:
        1. File Access: Attempts to open the specified file in read mode.
        2. Parsing: Uses PyYAML's safe_load to parse the YAML structure securely.
        3. Validation: Catches file existence errors and parsing errors.

    Outputs:
        Dict[str, Any]: A nested dictionary containing the study configuration.
                        Returns an empty dictionary if the file is not found.

    Raises:
        TypeError: If filepath is not a string.
        yaml.YAMLError: If the file contains invalid YAML syntax.
    """
    # Validate that the filepath input is strictly a string.
    if not isinstance(filepath, str):
        # Raise a TypeError if the input is invalid.
        raise TypeError(f"filepath must be a string, got {type(filepath)}.")

    try:
        # Open the file stream in read-only mode.
        with open(filepath, "r") as file:
            # Parse the YAML content safely into a Python dictionary.
            config = yaml.safe_load(file)

        # Log success to the console for auditability.
        print(f"\nSuccessfully loaded configuration from {filepath}")

        # Return the parsed configuration dictionary.
        return config

    except FileNotFoundError:
        # Handle the case where the file does not exist in the environment.
        # In a strict production environment, this would raise an error, but we handle it gracefully here.
        print(f"\nWarning: {filepath} not found. Please ensure the file exists in the working directory.")
        # Return an empty dictionary as a fallback.
        return {}
    except yaml.YAMLError as e:
        # Handle YAML parsing errors explicitly.
        print(f"\nError parsing YAML file {filepath}: {e}")
        # Re-raise the exception to halt execution.
        raise

# Load the configuration into memory.
# Note: Ensure 'config.yaml' is in your working directory with the content provided previously.
# For the sake of this self-contained example, if the file is missing, we will inject a mock config.
study_config = load_study_configuration()

# Inject a mock configuration if the file was not found to allow the example to proceed.
if not study_config:
    print("Injecting mock configuration for demonstration purposes...")
    study_config = {
        "microstructure_parameters": {
            "sampling_frequency_h_minutes": 1,
            "annualization_factor_T": 1/252,
            "expected_minutes_regular_session": 390,
            "trade_signing_baseline": "CLNV",
            "robustness_frequencies_h_minutes": [1, 5],
            "robustness_signing_methods": ["LR"],
            "session_timezone": "America/New_York",
            "binning_rule": "right_closed",
            "price_sampling_rule": "last_trade_at_or_before_grid_time",
            "max_quote_staleness_seconds": 5.0,
            "allowed_sale_conditions": ["REG"],
            "exclude_corrected_trades": True,
            "allowed_exchanges": "all_consolidated",
            "drop_locked_crossed_quotes": True
        },
        "filtering_selection_parameters": {
            "amihud_2002_price_floor": 5.0,
            "winsorization_percentile": 0.01,
            "winsorization_scope": "global_full_sample",
            "omit_nonpositive_price_impact_in_logs": True,
            "omit_nonpositive_omega_in_logs": True,
            "robustness_include_penny_stocks": False
        },
        "event_study_parameters": {
            "earnings_candidate_days": [-1, 0, 1],
            "earnings_window_radius_days": 22
        },
        "econometrics_parameters": {
            "ci_level": 0.95
        },
        "inflation_parameters": {
            "real_dollar_unit_scale_C": 1000000.0,
            "cpi_series": "CPI-U"
        },
        "economic_benchmarks": {
            "french_2008_fee_benchmark_as_share_mktcap": 0.0067
        },
        "risk_adjustment_parameters": {
            "sdf_entropy_habit_model_LM": 0.28,
            "sdf_entropy_jump_model_LM": 0.58,
            "sigma_omega_winsorized": 0.02,
            "sigma_omega_unwinsorized": 0.03
        },
        "expected_results_sanity_checks": {
            "mean_info_value_usd_millions_per_year_per_stock": 3.5,
            "mean_info_value_share_of_mktcap": 0.0004,
            "earnings_day_multiplier_exp_1p27": 3.57
        },
        "raw_data_schemas": {
            "df_raw_trades": {"columns": {"ticker": "str", "ts_val": "datetime", "price": "float", "size": "int"}},
            "df_raw_quotes": {"columns": {"ticker": "str", "ts_val": "datetime", "bid": "float", "ask": "float"}},
            "df_raw_meta": {"columns": {"permno": "int", "ticker": "str", "date": "date", "prev_close": "float", "market_equity": "float", "ln_me": "float", "momentum_2_12": "float", "cpi_multiplier": "float"}},
            "df_id_link": {"columns": {"ticker": "str", "permno": "int", "link_start_date": "date", "link_end_date": "date"}},
            "df_market_calendar": {"columns": {"date": "date", "session_open": "datetime", "session_close": "datetime"}},
            "df_agg_mktcap": {"columns": {"month": "date", "agg_market_cap": "float"}}
        }
    }

# ==============================================================================
# Execution of the End-to-End Study Pipeline
# ==============================================================================

if __name__ == "__main__":
    # Ensure we have valid inputs before running the master orchestrator.
    if raw_dataframes and study_config:
        
        print("\nInitiating 'The Value of Information' Replication Pipeline...")
        
        # Define the external dependency versions required for the ReplicationManifest.
        # This guarantees cryptographic reproducibility of the environment.
        external_versions = {
            "signing_method_version": "CLNV_v1.0",
            "taq_vendor_version": "WRDS_TAQ_2024",
            "calendar_source_version": "NYSE_CAL_v2",
            "cpi_series_version": "BLS_CPI_U_DEC24"
        }
        
        try:
            # Execute the master pipeline.
            # This will sequentially trigger Tasks 1 through 26, managing memory explicitly.
            master_artifacts = execute_master_replication_pipeline(
                raw_dataframes=raw_dataframes,
                raw_config=study_config,
                external_versions=external_versions,
                scenario_id="baseline_run"
            )
            
            # ==============================================================================
            # Inspecting the Outputs
            # ==============================================================================
            
            print("\n" + "="*80)
            print("PIPELINE EXECUTION COMPLETE")
            print("="*80)
            
            # 1. Accessing the Baseline Execution Results
            baseline_result = master_artifacts.get('baseline_execution')
            if baseline_result:
                # Extract the Table 1 Summary Statistics.
                df_table_1 = baseline_result.reporting_bundle.table_1_summary_stats
                print("\n[Table 1: Summary Statistics]")
                print(df_table_1.to_string(index=False))
                
                # Extract the Sanity Check Report.
                df_sanity = baseline_result.reporting_bundle.sanity_check_report
                print("\n[Sanity Check Report]")
                print(df_sanity[['Metric', 'Computed', 'Target', 'Passed']].to_string(index=False))
                
            # 2. Accessing the Risk Adjustment Bounds (The "Puzzle" Resolution)
            risk_bounds = master_artifacts.get('risk_adjustment_bounds')
            if risk_bounds:
                print("\n[Risk Adjustment Bounds (SDF Entropy)]")
                print(f"Empirical Risk-Neutral Value: {risk_bounds.empirical_omega_pct:.4f}%")
                print(f"Highest Admissible Risk-Adjusted Bound: {risk_bounds.highest_admissible_bound_pct:.4f}%")
                print(f"Active Management Fee Benchmark: {risk_bounds.fee_benchmark_pct:.4f}%")
                print(f"Puzzle Resolved by Risk Aversion? {risk_bounds.puzzle_resolved}")
                
            # 3. Accessing the Final Certification
            certification = master_artifacts.get('replication_certification')
            if certification:
                print("\n" + "="*80)
                print("FINAL REPLICATION CERTIFICATION")
                print("="*80)
                print(f"High Fidelity Achieved: {certification.high_fidelity_achieved}")
                print(f"Documentation Complete (Exact CIs Verified): {certification.documentation_complete}")
                
        except Exception as e:
            # Catch and log any fatal errors that occurred during pipeline execution.
            print(f"\nPipeline Execution Failed: {str(e)}")
            
    else:
        # Handle the case where the prerequisite data or configuration is missing.
        print("Error: Missing raw dataframes or configuration. Cannot proceed.")
```

## Output Structure

The pipeline returns a comprehensive dictionary containing six key artifacts:
-   **`baseline_execution`**: The `EndToEndExecutionResult` dataclass containing the `BaselineReportingBundle` (Tables 1-3, Figures 1-4) and the cryptographic `PipelineAuditLedger`.
-   **`robustness_signing`**: The `RobustnessSigningReport` detailing the sensitivity of the estimator to LR and EMO classification heuristics.
-   **`robustness_frequency`**: The `RobustnessFrequencyReport` detailing the convergence of the discrete approximation across varying $h$ intervals.
-   **`robustness_regression`**: The `RobustnessRegressionReport` containing exact finite-sample confidence intervals for all cross-sectional permutations.
-   **`risk_adjustment_bounds`**: The `RiskAdjustmentBoundReport` detailing the SDF entropy calculations that prove risk aversion cannot resolve the puzzle.
-   **`replication_certification`**: The `ReplicationCertificationReport` providing the final, objective boolean sign-off on the replication's fidelity.

## Project Structure

```
the_value_of_information/
│
├── the_value_of_information_draft.ipynb  # Main implementation notebook containing all 27 callables
├── config.yaml                           # Master configuration file (hyperparameters & filters)
├── requirements.txt                      # Python package dependencies
│
├── LICENSE                               # MIT Project License File
└── README.md                             # This file
```

## Customization

The pipeline is highly customizable via the `config.yaml` file. Researchers can modify study parameters such as:
-   **Temporal Discretization:** Adjust `sampling_frequency_h_minutes` to test the convergence of the discrete estimator at higher or lower resolutions.
-   **Trade Classification:** Modify `trade_signing_baseline` to utilize the Lee-Ready (LR) or Ellis-Michaely-O'Hara (EMO) algorithms instead of the CLNV baseline.
-   **Microstructure Filtering:** Adjust the `amihud_2002_price_floor` to evaluate the impact of penny-stock bid-ask bounce on the quadratic covariation estimator.
-   **Risk Bounding:** Modify the `sdf_entropy_jump_model_LM` to test alternative Stochastic Discount Factor specifications against the empirical fee benchmark.

## Contributing

Contributions are welcome. Please fork the repository, create a feature branch, and submit a pull request with a clear description of your changes. Adherence to PEP 8, strict type hinting (`typing` module), and the 1:1 inline comment-to-code-line ratio is strictly required for all pull requests to maintain the implementation-grade standard of this repository.

## Recommended Extensions

Future extensions, building upon this foundational framework, could include:
-   **Cross-Asset Application:** Applying the quadratic covariation estimator to alternative asset classes, such as cryptocurrency limit order books or high-frequency options markets, to evaluate informational efficiency outside of US equities.
-   **Dynamic Price Impact Estimation:** Integrating machine learning techniques (e.g., recurrent neural networks) to dynamically estimate the price impact coefficient ($\lambda_t$) across varying liquidity regimes, relaxing the linear pricing assumption.
-   **Behavioral Flow Isolation:** Developing advanced classification heuristics to explicitly isolate retail "attention-grabbing" trades from institutional flow, directly testing the Treynor (1971) "third type of trader" hypothesis proposed in the paper's conclusion.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Citation

If you use this code or the methodology in your research, please cite the original paper:

```bibtex
@article{kadan2026value,
  title={The Value of Information: A Puzzle},
  author={Kadan, Ohad and Manela, Asaf},
  journal={arXiv preprint arXiv:2605.11180},
  year={2026}
}
```

For the implementation itself, you may cite this repository:
```bibtex
@misc{chirinda2026informationbarometer,
  author = {Chirinda, Craig},
  title = {The Value of Information: An Empirical Replication Pipeline},
  year = {2026},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/chirindaopensource/the_value_of_information}}
}
```

## Acknowledgments

-   Credit to **Ohad Kadan** and **Asaf Manela** for the foundational theoretical work and empirical identification strategy that forms the entire basis for this computational replication.
-   This project is built upon the exceptional tools provided by the open-source community. Sincere thanks to the developers of the scientific Python ecosystem, particularly the **NumPy**, **Pandas**, **Statsmodels**, and **Linearmodels** contributors.

--

*This README was generated based on the structure and content of the `the_value_of_information_draft.ipynb` notebook and follows best practices for research software documentation.*

