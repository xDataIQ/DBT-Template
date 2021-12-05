[![Apache License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) ![dbt logo and version](https://img.shields.io/static/v1?logo=dbt&label=dbt-version&message=0.20.x&color=orange)

# {{ Package Name }}

This package models Salesforce data from [Fivetran's connector](https://fivetran.com/docs/applications/salesforce). It uses data in the format described by [this ERD](https://docs.google.com/presentation/d/1fB6aCiX_C1lieJf55TbS2v1yv9sp-AHNNAh2x7jnJ48/edit#slide=id.g3cb9b617d1_0_237).

The main focus of this package is enable users to better understand the performance of your opportunities. You can easily understand what is going on in your sales funnel and dig into how the members of your sales team are performing.

## Data Stack

- Loader: {{ your_data_loading_tools }}
- Warehouse: {{ your_warehouse }}
- Transformation: dbt
- Business Intelligence: {{ your_bi_tool }}

This package has been tested on BigQuery, Snowflake and Redshift.

## Models

The primary outputs of this package are described below. Staging and intermediate models are used to create these output models.

| **model**                          | **description**                                                                                                                                |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| salesforce\_\_manager_performance  | Each record represents a manager, enriched with data about their team's pipeline, bookings, losses, and win percentages.                       |
| salesforce\_\_owner_performance    | Each record represents an individual member of the sales team, enriched with data about their pipeline, bookings, losses, and win percentages. |
| salesforce\_\_sales_snapshot       | A single row snapshot that provides various metrics about your sales funnel.                                                                   |
| salesforce\_\_opportunity_enhanced | Each record represents an opportunity, enriched with related data about the account and opportunity owner.                                     |

## Installation Instructions

Check [dbt Hub](https://hub.getdbt.com/) for the latest installation instructions, or [read the dbt docs](https://docs.getdbt.com/docs/package-management) for more information on installing packages.

Include in your `packages.yml`

```yaml
packages:
  - package: fivetran/salesforce
    version: [">=0.4.0", "<0.5.0"]
```

## Configuration

By default, this package looks for your Salesforce data in the `salesforce` schema of your [target database](https://docs.getdbt.com/docs/running-a-dbt-project/using-the-command-line-interface/configure-your-profile). If this is not where your Salesforce data is, add the following configuration to your `dbt_project.yml` file:

```yml
# dbt_project.yml
---
config-version: 2

vars:
  salesforce_schema: your_schema_name
  salesforce_database: your_database_name
```

This package allows users to add additional columns to the opportunity enhanced table. Columns passed through must be present in the downstream source account table or user table. If you want to include a column from the user table, you must specify if you want it to be a field relate to the opportunity_manager or opportunity_owner.

```yml
# dbt_project.yml
---
vars:
  salesforce:
    opportunity_enhanced_pass_through_columns:
      [
        account_custom_field_1,
        account_custom_field_2,
        opportunity_manager.user_custom_column,
      ]
  salesforce_source:
    account_pass_through_columns:
      [account_custom_field_1, account_custom_field_2]
    user_pass_through_columns: [user_custom_column]
```

For additional configurations for the source models, visit the [Salesforce source package](https://github.com/fivetran/dbt_salesforce_source).

## Contributions

Additional contributions to this package are very welcome! Please create issues
or open PRs against `main`. Check out
[this post](https://discourse.getdbt.com/t/contributing-to-a-dbt-package/657)
on the best workflow for contributing to a package.

## Using This Project

<details>
  
  <summary>Developing in the Cloud IDE</summary>
  <p></p>
  
  The easiest way to contribute to this project is by developing in dbt Cloud. Contact {{ person_or_team_name }}. 
  If you need access, open a request in {{ tool_or_location }} by {{ best_way_to_write_request }}.
  
  Once you have access, navigate to the develop tab in the menu and fill out any required information to get connected.
  
  In the command line bar at the bottom of the interface, run the following commands one at a time:
  - `dbt deps`  - installs any packages defined in the packages.yml file.
  - `dbt seed`  - builds any .csv files as tables in the warehouse. These are located in the data folder of the project.
  - `dbt run`   - builds the models found in the project into your dev schema in the warehouse.
  
</details>

<details>
  
  <summary>Local Development</summary>
  <p></p>
  
  1. ### Install Requirements
      [Install dbt](https://docs.getdbt.com/dbt-cli/installation).   
      Optionally, you can [set up venv to allow for environment switching](https://discourse.getdbt.com/t/setting-up-your-local-dbt-run-environments/2353).

2. ### Setup

   Open your terminal and navigate to your `profiles.yml`. This is in the `.dbt` hidden folder on your computer, located in your home directory.

   On macOS, you can open the file from your terminal similar to this (which is using the Atom text editor to open the file):

   ```bash
   $ atom ~/.dbt/profiles.yml
   ```

   Insert the following into your `profiles.yml` file and change out the bracketed lines with your own information.
   [Here is further documentation](https://docs.getdbt.com/docs/available-adapters#dbt-labs-supported) for setting up your profile.

   ```yaml
   my_project:
     target: dev
     outputs:
       dev:
         type: [warehouse name]
         threads: 8
         account: [abc12345.us-west-1]
         user: [your_username]
         password: [your_password]
         role: transformer
         database: analytics
         warehouse: transforming
         schema: dbt_[your_name]
   ```

   | Configuration Key             | Definition                                                                                                                                                                                                              |
   | ----------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | my_project                    | This is defining a profile - this specific name should be the profile that is referenced in our dbt_project.yml                                                                                                         |
   | target: dev                   | This is the default environment that will be used during our runs.                                                                                                                                                      |
   | outputs:                      | This is a prompt to start defining targets and their configurations. You likely won't need more than `dev`, but this and any other targets you define can be used to accomplish certain functionalities throughout dbt. |
   | dev:                          | This is defining a target named `dev`.                                                                                                                                                                                  |
   | type: [warehous_name]         | This is the type of target connection we are using, based on our warehouse.                                                                                                                                             |
   | threads: 8                    | This is the amount of concurrent models that can run against our warehouse, for this user, at one time when conducting a `dbt run`                                                                                      |
   | account: [abc12345.us-west-1] | Change this out to the warehouse's account.                                                                                                                                                                             |
   | user: [your_username]         | Change this to use your own username that you use to log in to the warehouse                                                                                                                                            |
   | password: [your_password]     | Change this to use your own password for the warehouse                                                                                                                                                                  |
   | role: transformer             | This is the role that has the correct permissions for working in this project.                                                                                                                                          |
   | database: analytics           | This is the database name where our models will build                                                                                                                                                                   |
   | schema: dbt\_[your_name]      | Change this to a custom name. Follow the convention `dbt_[first initial][last_name]`. This is the schema that models will build into / test from when conducting runs locally.                                          |

3. ### Running dbt

   Run the following commands one at a time from your command line:

   - `dbt debug` - tests your connection. If this fails, check your profiles.yml.
   - `dbt deps` - installs any packages defined in the packages.yml file.
   - `dbt seed` - builds any .csv files as tables in the warehouse. These are located in the data folder of the project.
   - `dbt run` - builds the models found in the project into your dev schema in the warehouse.

</details>

## Resources:

- Provide [feedback](https://www.surveymonkey.com/r/DQ7K7WW) on our existing dbt packages or what you'd like to see next
- Have questions, feedback, or need help? Book a time during our office hours [here](https://calendly.com/fivetran-solutions-team/fivetran-solutions-team-office-hours) or email us at solutions@fivetran.com
- Find all of Fivetran's pre-built dbt packages in our [dbt hub](https://hub.getdbt.com/fivetran/)
- Learn how to orchestrate dbt transformations with Fivetran [here](https://fivetran.com/docs/transformations/dbt)
- Learn more about Fivetran overall [in our docs](https://fivetran.com/docs)
- Check out [Fivetran's blog](https://fivetran.com/blog)
- Learn more about dbt [in the dbt docs](https://docs.getdbt.com/docs/introduction)
- Check out [Discourse](https://discourse.getdbt.com/) for commonly asked questions and answers
- Join the [chat](http://slack.getdbt.com/) on Slack for live discussions and support
- Find [dbt events](https://events.getdbt.com) near you
- Check out [the dbt blog](https://blog.getdbt.com/) for the latest news on dbt's development and best practices
