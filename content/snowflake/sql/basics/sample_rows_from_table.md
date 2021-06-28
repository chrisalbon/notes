---
title: "Sample Random Rows From A Table"
author: "Chris Albon"
date: 2019-01-28T00:00:00-07:00
description: "How to sample a random set of rows from a table in Snowflake using SQL."
type: technical_note
draft: false
---

## Create Table Of Superheroes

{{< highlight sql >}}
-- Create a table called SUPERHEROES.
CREATE OR REPLACE TABLE SUPERHEROES (
  -- Column called ALTER_EGO allowing up to 100 characters
  "ALTER_EGO" VARCHAR(100),
  -- Column called BANK_BALANCE allowing 38 digits with 2 after the decimal point
  "AGE" NUMBER(3, 0)
);
{{< /highlight >}}

## Insert Rows For Each Superhero

{{< highlight sql >}}
-- Insert rows into SUPERHEROES
INSERT INTO SUPERHEROES 
    -- With the values
    VALUES
    ('The Bomber', '24'),
    ('Mr. Money', '12'),
    ('Nuke Miner', '59'),
    ('The Knife', '43'),
    ('Ninka Baker', '32'),
    ('Banana Bomber', '34'),
    ('Augustine', '12'),
    ('The Kid', '21'),
    ('The Viking', '291'),
    ('Skull Hustle', '10');
{{< /highlight >}}

## Sample Where Reach Row Has Chance Of Inclusion

{{< highlight sql >}}
-- Select all columns
SELECT *
-- From the SUPERHEROES table
FROM SUPERHEROES
-- Sample a subset of rows, where each row has a 25% chance of being selected
SAMPLE ROW (25);
{{< /highlight >}}
<table border=1>
    <thead>
        <tr>
            <th>ALTER_EGO</th>
            <th>AGE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Bomber</td>
            <td>24</td>
        </tr>
        <tr>
            <td>Nuke Miner</td>
            <td>59</td>
        </tr>
    </tbody>
</table>

## Sample Where N Rows Are Desired

{{< highlight sql >}}
-- Select all columns
SELECT *
-- From the SUPERHEROES table
FROM SUPERHEROES
-- Sample a subset of 3 rows
SAMPLE ROW (3 rows);
{{< /highlight >}}
<table border=1>
    <thead>
        <tr>
            <th>ALTER_EGO</th>
            <th>AGE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Augustine</td>
            <td>12</td>
        </tr>
        <tr>
            <td>Mr. Money</td>
            <td>12</td>
        </tr>
        <tr>
            <td>Ninka Baker</td>
            <td>32</td>
        </tr>
    </tbody>
</table>

## Sample With A Seed

Seeds improve repeatability, when rerun on the same data, samples using the same need value (e.g. `44`) will return the same rows.

{{< highlight sql >}}
-- Select all columns
SELECT *
-- From the SUPERHEROES table
FROM SUPERHEROES
-- Sample a subset of rows, where each row has a 25% chance of being selected with a seed of 99
SAMPLE ROW (25) SEED (99);
{{< /highlight >}}

<table border=1>
    <thead>
        <tr>
            <th>ALTER_EGO</th>
            <th>AGE</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>The Knife</td>
            <td>43</td>
        </tr>
    </tbody>
</table>
