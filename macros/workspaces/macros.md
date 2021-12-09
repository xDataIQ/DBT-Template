{% docs rpt_main_sales_management_cut_generator %}

Generates the "main" sales managements cuts for the passed columns. The dashboard where these cuts are being used can be seen [HERE](https://app.periscopedata.com/app/gitlab/828239/TD:-Sales-Funnel-Management-View---Preloaded-Cuts).

The "main" sales managements cuts display the following metrics:

1. Net ARR
1. New Logos
1. Pipeline Created
1. Sales Accepted Opportunities (SAOs)

For all these metrics the actual, the quarter to date / full quarter target, percentage of actual vs quarter to date / full quarter target are calculated.

{% enddocs %}

{% docs rpt_ratio_sales_management_cut_generator %}

Generates the "ratio" sales managements cuts for the passed columns. The dashboard where these cuts are being used can be seen [HERE](https://app.periscopedata.com/app/gitlab/828239/TD:-Sales-Funnel-Management-View---Preloaded-Cuts).

The "ratio" sales managements cuts display the following metrics:

1. Marketing Qualified Leads (MQLs)
1. Trials
1. Average Sales Price (ASP)
1. MQLs to SAOs
1. Win Rate

For all these metrics the actual, full quarter target, percentage of actual vs full quarter target are calculated.

For MQLs and Trials the quarter to date target and percentage of actual vs quarter to date target is also added.

{% enddocs %}
{% docs pte_base_query %}
Contains the bulk of the query which is used to estimate a customers likely to expand. 

Created with parameters to specify the model run type, the options being "training" or "scoring". The only difference in the output based on this parameter is in the end dates used to create the data set. 
{% enddocs %}