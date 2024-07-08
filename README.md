# Snap Metric Analyzer Using Gemini Pro/Azure OpenAI



## Overview

This project is an Metric SnapShot Scanner built using the Gemini Pro API/Azure OpenAI. It provides functionality to scan and fetch information from Snapshots and analyze the screenshot and uplods in Casandra DB which can be fetched based on the timestamp.

## Table of Contents

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
- [Usage](#usage)
- [Standalone](#standalone)
- [Pre-req](#pre-req)
- [DB Container Image and execution](#db-setup)
- [Files and Structure](#files-and-structure)

## Getting Started

### Prerequisites

- Python 3.10
- Gemini Pro API Key (Add instructions on how to obtain it)
- Azure OpenAI configuration as env.py

### Installation

1. Clone the repository:

    ```bash
    git clone https://github.kyndryl.net/Innovation-Council/snap-metric-analyzer.git
    cd snap-metric-analyzer
    ```

2. Install dependencies:

    ```bash
    pip install -r requirements.txt
    ```

3. Set up API key:

    - Obtain your Gemini Pro API key from [Gemini Pro Developer Portal]([gemini-pro-api-link](https://makersuite.google.com/app/apikey)).
    - Store the API key securely. (Consider using environment variables or a configuration file)
    - Obtain Azure OPENAI configuration

## Usage

### Standalone

Describe how to use your project. Provide examples and explain any configuration settings. Include information on how to run the application and any additional steps users need to take.

```bash
streamlit run app_snapMetricAnalyze.py
```

## Pre-req
Casandra DB needs to be setup

### DB Container Image and execution

```bash
docker run --name cassandra-container -d cassandra:latest
cqlsh
CREATE TABLE image_keyspace.images (
    image_id uuid PRIMARY KEY,
    description text,
    image_data blob,
    image_name text,
    image_timestamp timestamp,
    mime_type text,
    upload_timestamp timestamp
) WITH additional_write_policy = '99p'
    AND bloom_filter_fp_chance = 0.01
    AND caching = {'keys': 'ALL', 'rows_per_partition': 'NONE'}
    AND cdc = false
    AND comment = ''
    AND compaction = {'class': 'org.apache.cassandra.db.compaction.SizeTieredCompactionStrategy', 'max_threshold': '32', 'min_threshold': '4'}
    AND compression = {'chunk_length_in_kb': '16', 'class': 'org.apache.cassandra.io.compress.LZ4Compressor'}
    AND memtable = 'default'
    AND crc_check_chance = 1.0
    AND default_time_to_live = 0
    AND extensions = {}
    AND gc_grace_seconds = 864000
    AND max_index_interval = 2048
    AND memtable_flush_period_in_ms = 0
    AND min_index_interval = 128
    AND read_repair = 'BLOCKING'
    AND speculative_retry = '99p'

```

### APP Container Image and execution
```bash
docker run --name cassandra-container -d cassandra:latest
docker build -t snap-metric-analyze .
docker run -d --name snap-metric-container -e GOOGLE_API_KEY="" -p 8501:8501 snap-metric-analyze

```

### Docker-compose execution
```bash
docker-compose up --build -d

```



### Setup Cassandra db setup with Schema
```bash
You might receive error "NoHostAvailable" please follow the below to get rid of the error
Wait for a while till all containers are up
Execute docker exec -it cassandra-container cqlsh -f /docker-entrypoint-initdb.d/init-db.sql

```
## Files and Structure

- **Dockerfile**: For Building Docker Image to run as a container
- **README.md**: Project documentation.
- **app_snapMetricAnalyze.py**: Main application file.
- **requirements.txt**: List of Python packages required for the project.
- **env.py**: All LLM configuration
- **docker-compose.yml**: Docker compose to run all containers together


