# DM3 Metrics Collection Service

Gaining insight into the usage of the DM3 network is important to understand it's impact and raise funds.
With this in mind, the DM3 team is operating a metrics collection and presentation service. This repository contains the configuration for the service, making it easy for others to setup similar services.

## Basic Architecture

We collect metrics using Telegraf, and store them in InfluxDB, a time series database.

### How to setup

Currently, there is automated setup for the service. All changes need to be done on the server or uploaded, and then docker needs a restart. Some useful hints and commands:

#### Restart docker

```
cd <path to .env and docker-compose.yml>
docker compose down
docker compose --env-file .env up -d
```

#### Update telegraf.conf

Add a delivery service to the list of services we scrape in telegraf.conf by extending this array:

```
[[inputs.http]]
  urls = ["https://staging.dm3.network/ds/metrics", "https://testing.dm3.network/ds/metrics"]
```

Check any changes to the configuration by running:

```
telegraf --config telegraf.conf --test
```

If you don't have telegraf installed, you can run this instead:

```
docker run -it  -v $(pwd)/telegraf.conf:/telegraf.conf:ro telegraf:latest telegraf --config /telegraf.conf --test
```

## Data sources

## Delivery services

A metrics endpoint was recently added to our reference implementation of a delivery service. It collects:

- message count
- total message size
- notification count

For more information, please visit the documentation.
We scrape this data from services we know or find. We plan to automatically discover services in the network and add them to the list of services we scrape.
Please note that each service operator can decided whether or not to collect and publish metrics.

## On-chain profiles

Profiles published to ENS are public by default. We plan to count them, but are not doing so yet. There are 2 types of profiles:

1. Delivery service profiles
2. User profiles

We plan to count them, but are not doing so yet.
