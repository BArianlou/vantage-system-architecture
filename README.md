# Vantage System Architecture
Technical documentation and architectural specifications for the Vantage Predictive Engine (SPIPA).
![Method](https://img.shields.io/badge/Method-Monte_Carlo-blue)
![Build Status](https://github.com/BArianlou/vantage-nfl-arbitrage/actions/workflows/vantage_ci.yml/badge.svg)
![Python](https://img.shields.io/badge/python-3.10%2B-blue)
![Status](https://img.shields.io/badge/status-Operational-green)
# Project Vantage: Quantitative NFL Arbitrage Engine

**Version:** 1.1 (Dual-Vector)
**Architect:** Bijan Arianlou
**Status:** Operational // Private R&D

## 1. Executive Summary
Project Vantage is a high-frequency sports analytics engine designed to identify pricing inefficiencies (arbitrage) in NFL betting markets. Unlike traditional predictive models which rely on narrative or "power rankings," Vantage utilizes **physics-based metrics** (Expected Points Added - EPA) and **Monte Carlo simulations** to generate a "True Spread" and "True Total."

The system operates on the **Triune Architecture**:
1.  **Apex (Ingestion):** Pulls granular play-by-play data (45k+ rows) to build statistical profiles.
2.  **Chronos (Simulation):** Executes 10,000 vectorized match simulations to map the probability distribution of outcomes.
3.  **Vantage (Scanner):** Compares the model's probabilistic output against Vegas lines to detect actionable edges (>2.5 points).

## 2. System Architecture

![System Architecture](VANTAGE%20ARCHITECTURE.png)

## 3. Core Capabilities

### A. The Ingestion Layer (Apex)
*   **Source:** nflverse (Official NFL API scraper).
*   **Filtering:** Removes garbage time, penalties, and non-predictive plays.
*   **Metric:** Calculates EPA/Play (Efficiency) and Success Rate (Consistency) for every team.

### B. The Simulation Core (Chronos)
*   **Method:** Vectorized Monte Carlo (Numpy).
*   **Volume:** 10,000 iterations per run (<3 seconds execution time).
*   **Output:** Generates a full probability curve for both Spread (Winner) and Total (Over/Under).

### C. The Alpha Scanner
*   **Spread Threshold:** Triggers signal if `|Model - Vegas| > 2.5`.
*   **Total Threshold:** Triggers signal if `|Model - Vegas| > 3.0`.
*   **Confidence Tiering:**
    *   *Speculative:* < 1.0 point edge.
    *   *Lean:* 1.0 - 3.0 point edge.
    *   *Strong Signal:* > 3.0 point edge.

### 4. Performance (Super Bowl LX Benchmark)
*   **Matchup:** NE vs SEA
*   **Vegas Line:** NE -3.5 / Total 47.5
*   **Vantage Output:** Identified 15-point edge on the Total (Under).
*   **Latency:** Full pipeline execution in <45 seconds.

















