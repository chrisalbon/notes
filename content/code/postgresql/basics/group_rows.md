---
title: "Group Rows"
author: "Chris Albon"
date: 2018-06-17T00:00:00-07:00
description: "How to group rows in an SQL database."
type: technical_note
draft: false
aliases: [/postgresql/basics/group_rows/]
---

## Create Table

{{< highlight sql >}}
-- Create table called adventurers
CREATE TABLE adventurers (
    -- string variable
    name varchar(255),
    -- integer variable
    age int,
    -- string variable
    race varchar(255),
    -- string variable
    weapon varchar(255)
)
{{< /highlight >}}

## Insert Rows

{{< highlight sql >}}
-- Insert into the table adventurers
INSERT INTO adventurers (name, age, race, weapon)
VALUES ('Fjoak Doom-Wife', 28, 'Human', 'Axe'),
       ('Alooneric Cortte', 29, 'Human', 'Bow'),
       ('Piperel Ramsay', 35, 'Elf', 'Sword'),
       ('Casimir Yardley', 14, 'Elf', 'Magic')
{{< /highlight >}}

## Group Rows

{{< highlight sql >}}
-- Retrieve the race and average age from the table
SELECT race, AVG(age) FROM adventurers
-- Grouped by race
GROUP BY race, weapon
-- Where the weapon of the adventurer is a bow
HAVING weapon = 'Bow'
{{< /highlight >}}
<table border="1" style="border-collapse:collapse">
<tr><th>race</th><th>avg</th></tr>
<tr><td>Elf</td><td>14</td></tr>
<tr><td>Human</td><td>29</td></tr></table>