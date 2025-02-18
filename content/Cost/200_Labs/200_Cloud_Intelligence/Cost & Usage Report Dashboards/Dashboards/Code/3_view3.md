---
date: 2020-07-26T11:16:08-04:00
chapter: false
weight: 999
hidden: FALSE
---



## View 3 - Compute Savings Plan Eligible Spend
This view will be used to create the **Compute Savings Plan Eligible Spend** dashboard page.


### Create View
- {{%expand "Click here - if you have both Savings Plans and Reserved Instances" %}}

Modify the following SQL query for View3 - Compute Savings PlaneEligible spend: 
 - Update line 22, replace (database).(tablename) with your CUR database and table name 

		
		CREATE OR REPLACE VIEW "compute_savings_plan_eligible_spend" AS 
		SELECT DISTINCT
		"year"
		, "month"
		, "bill_payer_account_id" "payer_account_id"
		, "line_item_usage_account_id" "linked_account_id"
		, "bill_billing_period_start_date" "billing_period"
		, "date_trunc"('hour', "line_item_usage_start_date") "usage_date"
		, "sum"(CASE
			WHEN (((("line_item_line_item_type" = 'Usage') AND (NOT ("line_item_usage_type" LIKE '%Spot%'))) AND ("product_servicecode" <> 'AWSDataTransfer')) AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) THEN
				CASE
					WHEN (("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) THEN "line_item_unblended_cost"
					WHEN ("line_item_usage_type" LIKE '%Fargate%') THEN "line_item_unblended_cost"
					ELSE 0
				END
			ELSE 0 
			END) "unblended_cost"
		FROM
		(database).(tablename)
		WHERE (("bill_billing_period_start_date" >= ("date_trunc"('month', current_timestamp) - INTERVAL  '1' MONTH)) AND ("line_item_usage_start_date" < ("date_trunc"('day', current_timestamp) - INTERVAL  '1' DAY)) AND ((("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) OR ("line_item_usage_type" LIKE '%Fargate%')) AND ("line_item_line_item_type" = 'Usage') AND ("line_item_usage_type" NOT LIKE '%Spot%') AND ("product_servicecode" <> 'AWSDataTransfer') AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) 
		GROUP BY 1, 2, 3, 4,5,6
		


{{% /expand%}}



- {{%expand "Click here - if you have Savings Plans, but do not have Reserved Instances" %}}
If your usage changes you can delete and recreate the required view with Savings Plans or Reserved Instance usage.


Modify the following SQL query for View3 - Compute Savings PlaneEligible spend: 
 - Update line 22, replace (database).(tablename) with your CUR database and table name 

		CREATE OR REPLACE VIEW "compute_savings_plan_eligible_spend" AS 
		SELECT DISTINCT
		"year"
		, "month"
		, "bill_payer_account_id" "payer_account_id"
		, "line_item_usage_account_id" "linked_account_id"
		, "bill_billing_period_start_date" "billing_period"
		, "date_trunc"('hour', "line_item_usage_start_date") "usage_date"
		, "sum"(CASE
			WHEN (((("line_item_line_item_type" = 'Usage') AND (NOT ("line_item_usage_type" LIKE '%Spot%'))) AND ("product_servicecode" <> 'AWSDataTransfer')) AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) THEN
				CASE
					WHEN (("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) THEN "line_item_unblended_cost"
					WHEN ("line_item_usage_type" LIKE '%Fargate%') THEN "line_item_unblended_cost"
					ELSE 0
				END
			ELSE 0 
			END) "unblended_cost"
		FROM
		(database).(tablename)
		WHERE (("bill_billing_period_start_date" >= ("date_trunc"('month', current_timestamp) - INTERVAL  '1' MONTH)) AND ("line_item_usage_start_date" < ("date_trunc"('day', current_timestamp) - INTERVAL  '1' DAY)) AND ((("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) OR ("line_item_usage_type" LIKE '%Fargate%')) AND ("line_item_line_item_type" = 'Usage') AND ("line_item_usage_type" NOT LIKE '%Spot%') AND ("product_servicecode" <> 'AWSDataTransfer') AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) 
		GROUP BY 1, 2, 3, 4,5,6
		
{{% /expand%}}




- {{%expand "Click here - if you have Reserved Instances, but do not have Savings Plans" %}}
If your usage changes you can delete and recreate the required view with Savings Plans or Reserved Instance usage.

Modify the following SQL query for View3 - Compute Savings PlaneEligible spend: 
 - Update line 22, replace (database).(tablename) with your CUR database and table name 

		CREATE OR REPLACE VIEW "compute_savings_plan_eligible_spend" AS 
		SELECT DISTINCT
		"year"
		, "month"
		, "bill_payer_account_id" "payer_account_id"
		, "line_item_usage_account_id" "linked_account_id"
		, "bill_billing_period_start_date" "billing_period"
		, "date_trunc"('hour', "line_item_usage_start_date") "usage_date"
		, "sum"(CASE
			WHEN (((("line_item_line_item_type" = 'Usage') AND (NOT ("line_item_usage_type" LIKE '%Spot%'))) AND ("product_servicecode" <> 'AWSDataTransfer')) AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) THEN
				CASE
					WHEN (("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) THEN "line_item_unblended_cost"
					WHEN ("line_item_usage_type" LIKE '%Fargate%') THEN "line_item_unblended_cost"
					ELSE 0
				END
			ELSE 0 
			END) "unblended_cost"
		FROM
		(database).(tablename)
		WHERE (("bill_billing_period_start_date" >= ("date_trunc"('month', current_timestamp) - INTERVAL  '1' MONTH)) AND ("line_item_usage_start_date" < ("date_trunc"('day', current_timestamp) - INTERVAL  '1' DAY)) AND ((("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) OR ("line_item_usage_type" LIKE '%Fargate%')) AND ("line_item_line_item_type" = 'Usage') AND ("line_item_usage_type" NOT LIKE '%Spot%') AND ("product_servicecode" <> 'AWSDataTransfer') AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) 
		GROUP BY 1, 2, 3, 4,5,6
		


{{% /expand%}}




- {{%expand "Click here - if you do not have Reserved Instances, and do not have Savings Plans" %}}
If your usage changes you can delete and recreate the required view with Savings Plans or Reserved Instance usage.

Modify the following SQL query for View3 - Compute Savings PlaneEligible spend: 
 - Update line 22, replace (database).(tablename) with your CUR database and table name 

		CREATE OR REPLACE VIEW "compute_savings_plan_eligible_spend" AS 
		SELECT DISTINCT
		"year"
		, "month"
		, "bill_payer_account_id" "payer_account_id"
		, "line_item_usage_account_id" "linked_account_id"
		, "bill_billing_period_start_date" "billing_period"
		, "date_trunc"('hour', "line_item_usage_start_date") "usage_date"
		, "sum"(CASE
			WHEN (((("line_item_line_item_type" = 'Usage') AND (NOT ("line_item_usage_type" LIKE '%Spot%'))) AND ("product_servicecode" <> 'AWSDataTransfer')) AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) THEN
				CASE
					WHEN (("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) THEN "line_item_unblended_cost"
					WHEN (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) THEN "line_item_unblended_cost"
					WHEN ("line_item_usage_type" LIKE '%Fargate%') THEN "line_item_unblended_cost"
					ELSE 0
				END
			ELSE 0 
			END) "unblended_cost"
		FROM
		(database).(tablename)
		WHERE (("bill_billing_period_start_date" >= ("date_trunc"('month', current_timestamp) - INTERVAL  '1' MONTH)) AND ("line_item_usage_start_date" < ("date_trunc"('day', current_timestamp) - INTERVAL  '1' DAY)) AND ((("line_item_product_code" = 'AmazonEC2') AND ("line_item_operation" LIKE '%RunInstances%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-GB-Second%')) OR (("line_item_product_code" = 'AWSLambda') AND ("line_item_usage_type" LIKE '%Lambda-Provisioned-Concurrency%')) OR ("line_item_usage_type" LIKE '%Fargate%')) AND ("line_item_line_item_type" = 'Usage') AND ("line_item_usage_type" NOT LIKE '%Spot%') AND ("product_servicecode" <> 'AWSDataTransfer') AND ("line_item_usage_type" NOT LIKE '%DataXfer%')) 
		GROUP BY 1, 2, 3, 4,5,6
		

{{% /expand%}}

### Adding Cost Allocation Tags
{{% notice tip %}}
Cost Allocation tags can be added to any views. We recommend adding while creating the dashboard to eliminate rework. 
{{% /notice %}}

{{%expand "Click here - to add your cost allocation tags" %}}
To add your tags locate the the "line_item_usage_account_id" "linked_account_id" in the query you are using and add it after make sure to add a comma between each attribute and then add a group by field for any tags added (i.e. if you add one cost allocation tag you would add **,35** to group by in the bottom of your query)

- Example: Add your project tag by first locating the tag in your CUR attributes for project it will show up as **resource_tags_user_projects**. You will then find the **,"line_item_usage_account_id" "linked_account_id"** line in your query and add **, resource_tags_user_projects** then add **,35** in at the bottom of your query in the group by section.

{{% /expand%}}

### Validate View 
- Confirm the view is working, run the following Athena query and you should receive 10 rows of data:

        select * from costmaster.compute_savings_plan_eligible_spend
        limit 10
