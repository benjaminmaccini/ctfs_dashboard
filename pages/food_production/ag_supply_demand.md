---
title: Agricultural Supply and Demand
queries:
    - sources/ctfs/usda_census.sql
    - sources/ctfs/food_expenditures_normalized.sql
---


# Consumption vs. Production

```sql counties_exp
SELECT DISTINCT
    UPPER(County) AS county
FROM
    food_expenditures_normalized
```

<Dropdown
    name=selected_counties_exp
    data={counties_exp}
    value=county
    multiple=true
    selectAllByDefault=true
/>

```sql years_exp
SELECT DISTINCT
    YEAR
FROM
    usda_census
```

<Dropdown
    name=selected_years_exp
    data={years_exp}
    value=year
    multiple=true
    selectAllByDefault=true
/>

```sql sales_exp
SELECT
    COMMODITY_DESC,
    YEAR,
    SUM(CAST(usda_census.VALUE AS DECIMAL)) AS total
FROM
    usda_census    
WHERE
    COMMODITY_DESC IN ['CATTLE', 'POULTRY TOTALS', 'FRUIT & TREE NUT TOTALS', 'FIELD CROPS, OTHER', 'VEGETABLE TOTALS', 'HOGS', 'GRAIN', 'BERRY TOTALS', 'COTTON', 'MILK', 'SHEEP & GOATS TOTALS']
    AND COUNTY_NAME IN ${inputs.selected_counties_exp.value}
    AND YEAR IN ${inputs.selected_years_exp.value}
    AND NOT CONTAINS(usda_census.VALUE, '(')
    AND UNIT_DESC = '$'
GROUP BY
    COMMODITY_DESC,
    YEAR
```

```sql consumption
SELECT
    UPPER(County) AS county,
    expense_category,
    SUM(amount) as total
FROM
    food_expenditures_normalized
WHERE
    UPPER(County) IN ${inputs.selected_counties_exp.value}
    AND food_expenditures_normalized.source='total'
    AND expense_category NOT IN ['TOTAL', 'TOTAL_HOME', 'TOTAL_AWAY']
GROUP BY
    expense_category,
    County
```

```sql consumption_human
SELECT
    UPPER(County) AS county,
    expense_category,
    SUM(amount) as total
FROM
    food_expenditures_normalized
WHERE
    UPPER(County) IN ${inputs.selected_counties_exp.value}
    AND food_expenditures_normalized.source='human'
    AND expense_category NOT IN ['TOTAL', 'TOTAL_HOME', 'TOTAL_AWAY']
GROUP BY
    expense_category,
    County
```

<Tabs id="consumption">
    <Tab label="Total Consumption Per Household">
        <BarChart 
            data={consumption}
            x=county
            y=total
            yFmt=usd1k
            series=expense_category
            swapXY=true
        />
    </Tab>
    <Tab label="Human Consumption Per Household">
        <BarChart 
            data={consumption_human}
            x=county
            y=total
            yFmt=usd1k
            series=expense_category
            swapXY=true
        />
    </Tab>
</Tabs>

<Alert>
Knowing the unmet need for local agricultural products helps highlight the potential for increasing local food production in Central Texas. Research suggests that Central Texas’ production cannot realistically meet its total demand for food, but the charts below help highlight opportunities for increasing production. The chart below on the left shows the estimated consumption of agricultural products in Central Texas. The chart below on the right shows the amount locally produced for the same categories of agricultural products. Note that this dashboard only includes food grown for human consumption, not all agricultural production. Comparing sales volume is an imperfect measure because it does not indicate the quantity of food that was purchased or sold and does not consider differences between agricultural sale prices and consumer prices.

Highlights: Sales of food for human consumption are highest in McLennan County, followed by Falls and Milam counties. Most of these sales are generated from animal proteins rather than produce. While overall production exceeds consumption in over half of Central Texas counties, as a region, sales meet only 27.5% of consumption needs. This is due to large gaps in the counties with the highest consumption, including Travis, Williamson, and Hays counties.

Both charts are shown on the same scale to highlight the gap between production and consumption in Central Texas. This gap implies that most food consumed in Central Texas is produced outside the region. This means that much of the food supply is subject to supply chain disruptions outside the region and travels further before it reaches its final customer, adding additional emissions to its carbon footprint.

Note that the consumption chart includes food beyond what is produced locally. The data do not indicate that production is the only issue contributing to mismatches in supply and demand; distribution, waste, and economic and physical access to food should also be considered.
</Alert>

# Agricultural Sales By Product Type

```sql counties_sales
SELECT DISTINCT
    COUNTY_NAME
FROM
    usda_census
```

<Dropdown
    name=selected_counties_sales
    data={counties_sales}
    value=county_name
    multiple=true
    selectAllByDefault=true
/>

```sql years_sales
SELECT DISTINCT
    YEAR
FROM
    usda_census
```

<Dropdown
    name=selected_years_sales
    data={years_sales}
    value=year
    multiple=true
    selectAllByDefault=true
/>

```sql sales
SELECT
    COMMODITY_DESC,
    YEAR,
    SUM(CAST(usda_census.VALUE AS DECIMAL)) AS total
FROM
    usda_census    
WHERE
    COMMODITY_DESC IN ['CATTLE', 'POULTRY TOTALS', 'FRUIT & TREE NUT TOTALS', 'FIELD CROPS, OTHER', 'VEGETABLE TOTALS', 'HOGS', 'GRAIN', 'BERRY TOTALS', 'COTTON', 'MILK', 'SHEEP & GOATS TOTALS']
    AND COUNTY_NAME IN ${inputs.selected_counties_sales.value}
    AND YEAR IN ${inputs.selected_years_sales.value}
    AND NOT CONTAINS(usda_census.VALUE, '(')
    AND UNIT_DESC = '$'
GROUP BY
    COMMODITY_DESC,
    YEAR
```

```sql operations
SELECT
    COMMODITY_DESC,
    YEAR,
    SUM(CAST(usda_census.VALUE AS DECIMAL)) AS total
FROM
    usda_census    
WHERE
    COMMODITY_DESC IN ['CATTLE', 'POULTRY TOTALS', 'FRUIT & TREE NUT TOTALS', 'FIELD CROPS, OTHER', 'VEGETABLE TOTALS', 'HOGS', 'GRAIN', 'BERRY TOTALS', 'COTTON', 'MILK', 'SHEEP & GOATS TOTALS']
    AND COUNTY_NAME IN ${inputs.selected_counties_sales.value}
    AND YEAR IN ${inputs.selected_years_sales.value}
    AND NOT CONTAINS(usda_census.VALUE, '(')
    AND UNIT_DESC = 'OPERATIONS'
GROUP BY
    COMMODITY_DESC,
    YEAR
```

<Tabs id="ag-totals">
    <Tab label="Sales">
        <BarChart 
            data={sales}
            x=COMMODITY_DESC
            y=total
            yFmt=usd1m
            series=YEAR
            type=grouped
            swapXY=true
        />
    </Tab>
    <Tab label="Number of Operations">
        <BarChart 
            data={operations}
            x=COMMODITY_DESC
            y=total
            series=YEAR
            type=grouped
            swapXY=true
        />
    </Tab>
</Tabs>


<Alert>
Knowing the unmet need for local agricultural products helps highlight the potential for increasing local food production in Central Texas. Research suggests that Central Texas’ production cannot realistically meet its total demand for food, but the charts below help highlight opportunities for increasing production. The chart below on the left shows the estimated consumption of agricultural products in Central Texas. The chart below on the right shows the amount locally produced for the same categories of agricultural products. Note that this dashboard only includes food grown for human consumption, not all agricultural production. Comparing sales volume is an imperfect measure because it does not indicate the quantity of food that was purchased or sold and does not consider differences between agricultural sale prices and consumer prices.

Highlights: Sales of food for human consumption are highest in McLennan County, followed by Falls and Milam counties. Most of these sales are generated from animal proteins rather than produce. While overall production exceeds consumption in over half of Central Texas counties, as a region, sales meet only 27.5% of consumption needs. This is due to large gaps in the counties with the highest consumption, including Travis, Williamson, and Hays counties.

Both charts are shown on the same scale to highlight the gap between production and consumption in Central Texas. This gap implies that most food consumed in Central Texas is produced outside the region. This means that much of the food supply is subject to supply chain disruptions outside the region and travels further before it reaches its final customer, adding additional emissions to its carbon footprint.

Note that the consumption chart includes food beyond what is produced locally. The data do not indicate that production is the only issue contributing to mismatches in supply and demand; distribution, waste, and economic and physical access to food should also be considered.
</Alert>