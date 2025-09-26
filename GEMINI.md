## Directory Overview

This directory is a playground for using the `gemini-cli` tool, with a specific focus on interacting with Google BigQuery. It contains the necessary setup instructions and context for using `gemini-cli` to perform data analytics tasks on BigQuery.

## Usage

The primary purpose of this directory is to provide a configured environment for running the `gemini` command-line tool.

To find information about Mozilla data infrastructure and data in BigQuery use [docs.telemetry.mozilla.org](https://docs.telemetry.mozilla.org/).

Use bigquery-data-analytics extension to search and analyze data in BigQuery. When searching, prefer `mozdata` project which contains user-facing views over `moz-fx-data-shared-prod` which contains raw tables.
Most tables are partitioned by day and require a date filter in the query. Also use `sample_id` filter to limit the number of rows processed.
