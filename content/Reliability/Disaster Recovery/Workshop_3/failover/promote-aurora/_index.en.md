+++
title = "Promote Aurora"
date =  2021-05-11T11:43:28-04:00
weight = 6
+++

Amazon Aurora Global Database is designed for globally distributed applications, allowing a single Amazon Aurora database to span multiple AWS regions. It replicates your data with no impact on database performance, enables fast local reads with low latency in each region, and provides disaster recovery from region-wide outages. In disaster recovery situations, you can promote a secondary region to take full read-write responsibilities in under a minute.

Now let us promote the Amazon Aurora MySQL Secondary instance to a standalone instance.

1.1 Change your [console](https://us-west-1.console.aws.amazon.com/console)’s region to us-west-1 using the Region Selector in the upper right corner.

1.2 Navigate to **RDS** in the console.

{{< img a-1.png >}}

1.3 Click into **DB Instances**.

{{< img a-2.png >}}

1.4 Find **dr-immersionday-secondary-warm** instance and click **Actions**. Next click the **Remove from global database** option to promote the instance to a standalone database.

{{< img a-3.png >}}

1.5 Click **Remove and Promote** to confirm the server promotion.

{{< img a-4.png >}}

1.6 Grab a snack! It takes few minutes for the operation to complete.

## Congratulations! Your Amazon Aurora Secondary Database is now a standalone database and can become a primary database!
