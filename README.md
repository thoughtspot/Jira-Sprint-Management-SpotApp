# Jira Sprint Management SpotApp

SpotApps are ThoughtSpot’s out-of-the-box solution templates built for specific use cases and data sources. They utilize ThoughtSpot Modeling Language (TML) Blocks, which are pre-built pieces of code that are easy to download and implement directly from the product.

The Jira Sprint Management SpotApp mimics the Jira data model. When deployed, it creates several Worksheets, Answers, and Liveboards based on your Jira data in your cloud data warehouse.

## Prerequisites

Before you can deploy the Jira Sprint Management SpotApp, you must complete the following prerequisites:

- **Review Required Data**: Examine the required tables and columns for the SpotApp.
- **Ensure Column Compatibility**: Make sure that your columns match the required column type listed in the schema for your SpotApp.
- **Sync Data**: Synchronize all tables and columns from Jira to your cloud data warehouse. Though you can choose to sync only the required tables and columns, ThoughtSpot recommends syncing all tables and columns from Jira to your CDW. This includes Jira’s out-of-the-box columns or any custom columns you are using.
- **Collaborate on Data Movement**: If you are using an ETL/ELT tool or working with another team within your organization, sync all columns from the tables listed in the SpotApp.
- **Obtain Credentials**: Acquire credentials and SYSADMIN privileges to connect to your cloud data warehouse. The warehouse must contain the data you would like ThoughtSpot to use to create Answers, Liveboards, and Worksheets. Refer to the connection reference for your cloud data warehouse for information about required credentials.
- **Unique Connection Name**: Each new SpotApp connection name must be unique.
- **Administrator Access to Jira**: Maintain administrator access to manage Jira resources.
- **Access to Jira Tables**: Ensure access to the following Jira table in your cloud data warehouse. Refer to the Jira Sprint Management SpotApp schema for more details:
  - `JIRA_SPRINTS`

### Run SQL Commands

Execute the necessary SQL commands in your cloud data warehouse to standardize data types and column names. Modify the code as needed for the SQL requirements of your specific cloud data warehouse.

```sql
create or replace view "<DATABASE NAME>"."<SCHEMA NAME>"."JIRA_SPRINTS" as select
ID as Id,
NAME as Name,
GOAL as Goal,
BOARD_ID as OriginBoardId,
START_DATE as StartDate,
END_DATE as EndDate,
COMPLETE_DATE as CompleteDate from "<DATABASE NAME>"."<SCHEMA NAME>"."<SPRINT TABLE NAME>";

create or replace view "<DATABASE NAME>"."<SCHEMA NAME>"."JIRA_SPRINTS" as
select
ID as Id,
NAME as Name,
case
when "START_DATE" is not NULL and "COMPLETE_DATE" is NULL then 'active'
when "COMPLETE_DATE" is not NULL then 'closed'
when "START_DATE" is NULL and "END_DATE" is NULL and "COMPLETE_DATE" is NULL then 'future'
end as State,
GOAL as Goal,
BOARD_ID as OriginBoardId,
START_DATE as StartDate,
END_DATE as EndDate,
COMPLETE_DATE as CompleteDate
from "<DATABASE NAME>"."<SCHEMA NAME>"."<SPRINT TABLE NAME>";

Deployment
After completing the prerequisites, you are ready to deploy the Jira Sprint Management SpotApp and begin leveraging its pre-built content.

Implementation Steps
Once you have downloaded the Zip file and verified its contents, follow these steps:

Log into your ThoughtSpot instance and create an Embrace connection to all relevant views.
Import the TML for the worksheets and verify that it has all been imported without any errors.
Import the TML for the pinboard and verify that it has been imported without any errors.
You are ready to start searching your Jira data!
