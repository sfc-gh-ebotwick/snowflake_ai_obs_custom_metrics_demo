# snowflake_ai_obs_custom_metrics_demo
Use Snowflake's AI Observability framework to instrument and evaluate a simple GenAI app using custom metrics for evals!


Setup

```
-- Create DB, Schema, and Warehouse
CREATE DATABASE IF NOT EXISTS CUSTOM_METRIC_DEMO_DB;
CREATE SCHEMA IF NOT EXISTS DATA;
CREATE WAREHOUSE IF NOT EXISTS MEDIUM WITH WAREHOUSE_SIZE='MEDIUM';

-- Create an API integration with Github
CREATE OR REPLACE API INTEGRATION GIT_INTEGRATION_EBOTWICK
   api_provider = git_https_api
   api_allowed_prefixes = ('https://github.com/sfc-gh-ebotwick')
   enabled = true
   comment='Git integration with Elliott Botwick''s repositories';

-- Create the integration with the Github demo repository
CREATE OR REPLACE GIT REPOSITORY CUSTOM_METRIC_DEMO_REPO
   ORIGIN = 'https://github.com/sfc-gh-ebotwick/snowflake_ai_obs_custom_metric_demo.git' 
   API_INTEGRATION = 'GIT_INTEGRATION_EBOTWICK' 
   COMMENT = 'Github Repository ';

-- Fetch most recent files from Github repository
ALTER GIT REPOSITORY CUSTOM_METRIC_DEMO_REPO FETCH;

-- Copy AI Obs notebook into snowflake configure runtime settings
CREATE OR REPLACE NOTEBOOK CUSTOM_METRIC_DEMO_DB.DATA.AI_OBS_CUSTOM_METRICS_DEMO
FROM '@CUSTOM_METRIC_DEMO_DB.DATA.CUSTOM_METRIC_DEMO_REPO/branches/main/' 
MAIN_FILE = 'AI_OBS_CUSTOM_METRICS_DEMO.ipynb' QUERY_WAREHOUSE = MEDIUM
RUNTIME_NAME = 'SYSTEM$BASIC_RUNTIME' 
IDLE_AUTO_SHUTDOWN_TIME_SECONDS = 3600;
```
