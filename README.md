# Gemini CLI playground

## Install gemini-cli
See https://github.com/google-gemini/gemini-cli

```
export GOOGLE_CLOUD_PROJECT="mozdata-nonprod"
export GOOGLE_CLOUD_LOCATION="us-west1"
```
To start:
```
gemini
```

## Extensions for BigQuery
See https://cloud.google.com/bigquery/docs/develop-with-gemini-cli

Install extensions:
```
gemini extensions install https://github.com/gemini-cli-extensions/bigquery-data-analytics
gemini extensions install https://github.com/gemini-cli-extensions/bigquery-conversational-analytics
```

Install [MCP Toolbox for Databases](https://googleapis.github.io/genai-toolbox/getting-started/introduction/#why-toolbox):
```
curl -L -o ~/.gemini/extensions/bigquery-data-analytics/toolbox \
  https://storage.googleapis.com/genai-toolbox/v0.16.0/darwin/arm64/toolbox
curl -L -o ~/.gemini/extensions/bigquery-conversational-analytics/toolbox \
  https://storage.googleapis.com/genai-toolbox/v0.16.0/darwin/arm64/toolbox
chmod +x ~/.gemini/extensions/bigquery-*/toolbox
```
See https://github.com/googleapis/genai-toolbox/releases for latest release.

TODO: some tool in conversational analytics extension is broken, disable it if gemini fails with `The GenerateContentRequest proto is invalid:\\n * tools[0].function_declarations[15].name: [FIELD_INVALID]`:
```
gemini extensions disable bigquery-conversational-analytics
```

## DataHub MCP server
_This might fail due to bad schema definition_

To use MCP server, in ~/.gemini/settings.yaml add:
```yaml
"mcpServers": {
    "dataHub": {
      "httpUrl": "https://mozilla.acryl.io/integrations/ai/mcp/?token={YOUR_API_TOKEN}"
    }
  }
```

## Interacting with BigQuery in Gemini CLI
Start CLI with:
```
export BIGQUERY_PROJECT="mozdata"
gemini
```

### Example
```
> where do I find FxA Glean telemetry?

(... some tool calls ...)

✦ FxA Glean telemetry data is available in BigQuery in the mozdata project. You can find the data in the following tables:

   * mozdata.accounts_frontend.events_stream
   * mozdata.accounts_backend.events_stream

  These tables are partitioned, so you need to include a WHERE clause on the submission_timestamp column in your queries. For example, to get the 10 most recent events
  from the accounts_frontend.events_stream table from the last 7 days, you can run the following query:

    1 SELECT
    2   event_timestamp,
    3   event_category,
    4   event_name,
    5   event_extra
    6 FROM
    7   `mozdata.accounts_frontend.events_stream`
    8 WHERE
    9   submission_timestamp > TIMESTAMP_SUB(CURRENT_TIMESTAMP(),
   10     INTERVAL 7 DAY)
   11 ORDER BY
   12   event_timestamp DESC
   13 LIMIT
   14   10

  Here are the results of running that query:

    1 {"event_category":"glean","event_extra": (...)
```
