---
title: "Quickstart for dbt Cloud and Databricks"
description: "Quickstart for dbt Cloud and Databricks."
id: "databricks"
sidebar_label: "Databricks quickstart"
---
For the Databricks project in the quickstart guide, you'll learn how to:

- Create a Databricks workspace.
- Load sample data into your Databricks account.
- Connect dbt Cloud to Databricks.
- Take a sample query and turn it into a model in your dbt project. A model in dbt is a select statement.
- Add tests to your models
- Document your models
- Schedule a job to run

:::tip Videos for you
You can check out [dbt Fundamentals](https://courses.getdbt.com/courses/fundamentals) for free if you're interested in course learning with videos.

:::

## Prerequisites​

- You have a  [dbt Cloud account](https://cloud.getdbt.com/). 
- You have an account with a cloud service provider (such as AWS, GCP, and Azure) and have permissions to create an S3 bucket with this account.

## Create a Databricks workspace

1. Use your existing account or sign up for a Databricks account at [Try Databricks](https://databricks.com/). Complete the form with your user information.
    
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/signup_form.png" title="Sign up for Databricks" />
    </div>

2. For the purpose of this tutorial, you will be selecting AWS as our cloud provider but if you use Azure or GCP internally, please choose one of them. The setup process will be similar.
3. Check your email to complete the verification process.
4. After setting up your password, you will be guided to choose a subscription plan. Select the `Premium` or `Enterprise` plan to access the SQL Compute functionality required for using the SQL warehouse for dbt. We have chosen `Premium` for this tutorial. Click **Continue** after selecting your plan.
    
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/choose_plan.png" title="Choose Databricks Plan" />
    </div>

5. Click **Get Started** when you come to this below page and then **Confirm** after you validate that you have everything needed.

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/validate_1.png" />
    </div>
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/validate_2.png" />
    </div>

6. Now it's time to create your first workspace. A Databricks workspace is an environment for accessing all of your Databricks assets. The workspace organizes objects like notebooks, SQL warehouses, clusters, etc into one place.  Provide the name of your workspace and choose the appropriate AWS region and click **Start Quickstart**. You might get the checkbox of `I have data in S3 that I want to query with Databricks`. You do not need to check this off for the purpose of this tutorial. 

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/setup_first_workspace.png" title="Setup First Workspace" />
    </div>

7. By clicking on `Start Quickstart`, you will be redirected to AWS and asked to log in if you haven’t already. After logging in, you should see a page similar to this. 

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/quick_create_stack.png" title="Create AWS resources" />
    </div>

:::tip
If you get a session error and don’t get redirected to this page, do not worry, go back to the Databricks UI and create a workspace from the interface. All you have to do is click **create workspaces**, choose the quickstart, fill out the form and click **Start Quickstart**.
:::

8. There is no need to change any of the pre-filled out fields in the Parameters. Just add in your Databricks password under **Databricks Account Credentials**.  Check off the Acknowledgement and click **Create stack**.   
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/parameters.png" title="Parameters" />
    </div>    

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/create_stack.png" title="Capabilities" />
    </div>    
9. Afterwards, you should land on the CloudFormation > Stacks page. Once the status becomes `CREATE_COMPLETE`, you will be ready to start. This process can take about 5 minutes so feel free to click refresh to refresh the status updates.     
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/stack_status.png" title="Confirm Status Completion" />
    </div>
10. Go back to the Databricks tab. You should see that your workspace is ready to use.
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/workspaces.png" title="A Databricks Workspace" />
    </div>
11. Now let’s jump into the workspace. Click **Open** and log into the workspace using the same login as you used to log into the account. 

## Load data

1. First we need a SQL warehouse. Find the drop down menu and toggle into the SQL space.
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/go_to_sql.png" title="SQL space" />
    </div>
2. We will be setting up a SQL warehouse now.  Select **SQL Warehouses** from the left hand side console.  You will see that a default SQL Warehouse exists.  
    <!--
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/sql_endpoints.png" title="SQL warehouses" /> 
    </div>
    -->
3. Click **Start** on the Starter Warehouse.  This will take a few minutes to get the necessary resources spun up.

4. While you're waiting, download the three CSV files locally that you will need for this tutorial. You can find them here:
    - [jaffle_shop_customers.csv](https://dbt-tutorial-public.s3-us-west-2.amazonaws.com/jaffle_shop_customers.csv)
    - [jaffle_shop_orders.csv](https://dbt-tutorial-public.s3-us-west-2.amazonaws.com/jaffle_shop_orders.csv)
    - [stripe_payments.csv](https://dbt-tutorial-public.s3-us-west-2.amazonaws.com/stripe_payments.csv)

5. Once the SQL Warehouse is up, click **Create** and then **Table** on the dropdown menu. 
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/create_table_using_databricks_SQL.png" title="Create Table Using Databricks SQL" />
    </div>

6. Let's load the Jaffle Shop Customers data first. Drop in the `jaffle_shop_customers.csv` file into the UI.
    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/databricks_table_loader.png" title="Databricks Table Loader" />
    </div>

7. Update the Table Attributes at the top:

    - <b>data_catalog</b> = hive_metastore
    - <b>database</b> = default 
    - <b>table</b> = jaffle_shop_customers
    - Make sure that the column data types are correct. The way you can do this is by hovering over the datatype icon next to the column name. 
        - <b>ID</b> = bigint
        - <b>FIRST_NAME</b> = string
        - <b>LAST_NAME</b> = string

    <div style={{maxWidth: '600px'}}>
    <Lightbox src="/img/databricks_tutorial/images/jaffle_shop_customers_upload.png" title="Load jaffle shop customers" />
    </div>

8. Click **Create** on the bottom once you’re done. 

9. Now let’s do the same for `Jaffle Shop Orders` and `Stripe Payments`. 

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/jaffle_shop_orders_upload.png" title="Load jaffle shop orders" />
    </div>

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/stripe_payments_upload.png" title="Load stripe payments" />
    </div>
    
10. Once that's done, make sure you can query the training data.  Navigate to the `SQL Editor` through the left hand menu.  This will bring you to a query editor.
11. Ensure that you can run a `select *` from each of the tables with the following code snippets. 

    ```sql
    select * from default.jaffle_shop_customers
    select * from default.jaffle_shop_orders
    select * from default.stripe_payments
    ```

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/query_check.png" title="Query Check" />
    </div>

12. To ensure any users who might be working on your dbt project has access to your object, run this command.

    ```sql 
    grant all privileges on schema default to users;
    ```

## Connect dbt Cloud to Databricks

 There are two ways to connect dbt Cloud to Databricks. The first option is Partner Connect, which provides a streamlined setup to create your dbt Cloud account from within your new Databricks trial account. The second option is to create your dbt Cloud account separately and build the Databricks connection yourself (connect manually). If you are looking to get started quickly, dbt Labs recommends using Partner Connect. If you are looking to customize your setup from the very beginning and gain familiarity with the dbt Cloud setup flow, we recommend connecting manually.


<Tabs>
<TabItem value="partner-connect" label="Use Partner Connect" default>

1. In the Databricks workspace, on the left-side console, click **Partner Connect**. 

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/databricks_partner_connect.png" title="Databricks Partner Connect" />
    </div>

2. Select the dbt tile under `Data preparation and transformation`.
3. Click **Next** when prompted to **Connect to partner**. This action will create a service principal, PAT token for that service principle, and SQL warehouse for the dbt Cloud account to use. This does mean that you will have two SQL warehouses at your disposal from the previous step and from using Partner Connect.

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/databricks_connect_to_partner.png" title="Databricks Partner Connect Connect to dbt Cloud" />
    </div>


4. Click **Connect to dbt Cloud**.

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/databricks_connect_to_dbt_cloud.png" title="Databricks Partner Connect Connect to dbt Cloud" />
    </div>

5. After the new tab loads, you will see a form. If you already created a dbt Cloud account, you will be asked to provide an account name. If you haven't created account, you will be asked to provide an account name and password. 

    <div style={{maxWidth: '400px'}}>
    <Lightbox src="/img/databricks_tutorial/images/databricks_partner_connect_create_account.png" title="Databricks Partner Connect Connect to dbt Cloud" />
    </div>

6. After you have filled out the form and clicked **Complete Registration**, you will be logged into dbt Cloud automatically. 

</TabItem>

<TabItem value="manual-connect" label="Connect manually">

#### Get endpoint and token information 
To manually set up dbt Cloud, you will need the SQL warehouse connection information and to generate a user token. You can find your SQL warehouse connection information by going to the **Databricks UI > SQL > SQL warehouses > Starter Endpoint > Connection details**. Save this information because you will need it later.

<Lightbox src="/img/databricks_tutorial/images/SQL_Endpoint_Details.png" title="Databrick SQL Endpoint Connection Information" />

To generate a user token for your development credentials in dbt Cloud, click **Settings** on the left side console (while still in the SQL part of the workspace). Click **Personal Access Token** and provide a comment like `dbt Cloud development`. Save the token information somewhere because you will need it for the next part. 
<div style={{maxWidth: '400px'}}>
<Lightbox src="/img/databricks_tutorial/images/Generate_user_token.png" title="Generate User Token" />
</div>

#### Connect to Databricks

1. Create a new project in [dbt Cloud](https://cloud.getdbt.com/). From **Account settings** (using the gear menu in the top right corner), click **+ New Project**.
2. Enter a project name and click **Continue**.
3. For the warehouse, click **Databricks** then **Next** to set up your connection.
    <Lightbox src="/img/databricks_tutorial/images/dbt_cloud_setup_databricks_connection_start.png" title="dbt Cloud - Choose Databricks Connection" /> 
4. For Databricks settings, reference your SQL warehouse connection details from step 6 of the previous section for each of the following fields:

    - Method will be ODBC
    - Hostname comes from Server hostname
    - Endpoint comes from the last part of HTTP path after `/endpoints`
      <div style={{maxWidth: '400px'}}>
      <Lightbox src="/img/databricks_tutorial/images/dbt_cloud_setup_databricks_connection_details.png" title="dbt Cloud - Databricks Workspace Settings" />
      </div>

5. For your Development Credentials, type:

    - `User` and `token` that you saved in a previous step.
    - You’ll notice that the schema name has been auto created for you. By convention, this is `dbt_<first-initial><last-name>`. This is the schema connected directly to your development environment, and it's where your models will be built when running dbt within the Cloud IDE.

6. Click **Test Connection**. This verifies that dbt Cloud can access your Databricks workspace.
7. Click **Continue** if the test succeeded. If it fails, you might need to check your Databricks settings and credentials.

</TabItem>
</Tabs>

## Set up a dbt Cloud managed repository 
<Snippet src="tutorial-managed-repo" />

## Initialize your dbt project​ and start developing
Now that you have a repository configured, you can initialize your project and start development in dbt Cloud:

1. Click **Develop** from the upper left. It might take a few minutes for your project to spin up for the first time as it establishes your git connection, clones your repo, and tests the connection to the warehouse.
2. Above the file tree to the left, click **Initialize your project**. This builds out your folder structure with example models.
3. Make your initial commit by clicking **Commit**. Use the commit message `initial commit`. This creates the first commit to your managed repo and allows you to open a branch where you can add new dbt code.
4. You can now directly query data from your warehouse and execute `dbt run`. You can try this out now:
    - In the IDE's editor, add this query: 
        ```sql
        select * from default.jaffle_shop_customers
        ```
    - In the command line bar at the bottom, enter `dbt run` and click **Enter**. 

## Build your first model
1. Click **Develop** from the upper left of dbt Cloud. You need to create a new branch since the main branch is now set to read-only mode. 
2. Click **Create branch**. You can name it `add-customers-model`.
3. Click **Develop** from the upper left of dbt Cloud.
4. Click the **...** next to the Models directory, then select **Create file**.  
5. Name the file `models/customers.sql`, then click **Create**.
6. Copy the following query into the file and click **Save File**.

```sql
with customers as (

    select
        id as customer_id,
        first_name,
        last_name

    from jaffle_shop_customers

),

orders as (

    select
        id as order_id,
        user_id as customer_id,
        order_date,
        status

    from jaffle_shop_orders

),

customer_orders as (

    select
        customer_id,

        min(order_date) as first_order_date,
        max(order_date) as most_recent_order_date,
        count(order_id) as number_of_orders

    from orders

    group by 1

),

final as (

    select
        customers.customer_id,
        customers.first_name,
        customers.last_name,
        customer_orders.first_order_date,
        customer_orders.most_recent_order_date,
        coalesce(customer_orders.number_of_orders, 0) as number_of_orders

    from customers

    left join customer_orders using (customer_id)

)

select * from final
```

7. Enter `dbt run` in the command prompt at the bottom of the screen. You should get a successful run and see three models under DETAILS.

Later, you can connect your business intelligence (BI) tools to these views and tables so they only read cleaned up data rather than raw data in your BI tool.

#### FAQs

<FAQ src="Runs/checking-logs" />
<FAQ src="Project/which-schema" />
<FAQ src="Models/create-a-schema" />
<FAQ src="Models/run-downtime" />
<FAQ src="Troubleshooting/sql-errors" />

## Change the way your model is materialized

<Snippet src="quickstarts/change-way-model-materialized" />

## Delete the example models

<Snippet src="quickstarts/delete-example-models" />

## Build models on top of other models

<Snippet src="quickstarts/build-models-atop-other-models" />

<Snippet src="quickstarts/test-and-document-your-project" />

<Snippet src="quickstarts/schedule-a-job" />