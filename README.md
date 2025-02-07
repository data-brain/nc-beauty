# NC Beauty

No code for this as it's mostly done through the console. Just some things to be aware of.

## Task 1:

When creating the tables I'd recommend doing it through the Athena console rather than the Glue console. Under the hood Athena tables seem to have some kind of default setting that omits dodgy data. Whereas a table created by Glue does not.

When querying the influencers table initially the output omits the dodgy follower count data:

![Athena influencer query](./images/querying_athena_influencer_table.png)

Querying the table created in the Glue console on the other hand:

![Glue influencer query](./images/querying_glue_influencer_table.png)

---

Query for getting influencer with higher than average followers:

```sql
SELECT *
FROM "athena_influencers_table"
WHERE followers > (
    SELECT avg(followers) from athena_influencers_table
);
```

## Task 2:

**I'd really recommend doing this with the Athena `Create Table` UI as it makes things easier. Doing it with Glue can lead to problems that are an arse to debug**

Creating the table was a bit of a mare. The instructions point you towards OpenCSVSerde which you can read about here: https://docs.aws.amazon.com/athena/latest/ug/csv-serde.html

This is the configuration needed to skip the first line:

```txt
"skip.header.line.count"="1"
```

### Screenshots Showing the Steps taken on Athena to create the table:

**STEP 1:**

Create table from existing metabase configuration - link to s3 bucket

![Step 1](./images/Athena_Skincare_Table/step_1.png)

**STEP 2:**

Specify file format and SerDe library (OpenCSVSerDe). Change the `seperatorChar`, `quoteChar` and `escapeChar` properties.

![Step 2](./images/Athena_Skincare_Table/step_2.png)

**STEP 3:**

Specify column names and data types.

![Step 3](./images/Athena_Skincare_Table/step_3.png)

**STEP 4:**

In optional table properties specify that to want to skip the header.

![Step 4](./images/Athena_Skincare_Table/step_4.png)

**STEP 5:**

This should produce a DDL statement shown in the preview box at the bottom. You can click the create table button or copy and paste it into the Athena editor.

![Step 5](./images/Athena_Skincare_Table/step_5.png)
