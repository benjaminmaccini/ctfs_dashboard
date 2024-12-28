---
title: Agricultural Supply and Demand
---

<Alert>
## Consumption vs. Production

Knowing the unmet need for local agricultural products helps highlight the potential for increasing local food production in Central Texas. Research suggests that Central Texas’ production cannot realistically meet its total demand for food, but the charts below help highlight opportunities for increasing production. The chart below on the left shows the estimated consumption of agricultural products in Central Texas. The chart below on the right shows the amount locally produced for the same categories of agricultural products. Note that this dashboard only includes food grown for human consumption, not all agricultural production. Comparing sales volume is an imperfect measure because it does not indicate the quantity of food that was purchased or sold and does not consider differences between agricultural sale prices and consumer prices.

Highlights: Sales of food for human consumption are highest in McLennan County, followed by Falls and Milam counties. Most of these sales are generated from animal proteins rather than produce. While overall production exceeds consumption in over half of Central Texas counties, as a region, sales meet only 27.5% of consumption needs. This is due to large gaps in the counties with the highest consumption, including Travis, Williamson, and Hays counties.

Both charts are shown on the same scale to highlight the gap between production and consumption in Central Texas. This gap implies that most food consumed in Central Texas is produced outside the region. This means that much of the food supply is subject to supply chain disruptions outside the region and travels further before it reaches its final customer, adding additional emissions to its carbon footprint.

Note that the consumption chart includes food beyond what is produced locally. The data do not indicate that production is the only issue contributing to mismatches in supply and demand; distribution, waste, and economic and physical access to food should also be considered.
</Alert>

```sql categories
  select
      category
  from needful_things.orders
  group by category
```

<Dropdown data={categories} name=category value=category>
    <DropdownOption value="%" valueLabel="All Categories"/>
</Dropdown>

<Dropdown name=year>
    <DropdownOption value=% valueLabel="All Years"/>
    <DropdownOption value=2019/>
    <DropdownOption value=2020/>
    <DropdownOption value=2021/>
</Dropdown>

```sql orders_by_category
  select 
      date_trunc('month', order_datetime) as month,
      sum(sales) as sales_usd,
      category
  from needful_things.orders
  where category like '${inputs.category.value}'
  and date_part('year', order_datetime) like '${inputs.year.value}'
  group by all
  order by sales_usd desc
```

<BarChart
    data={orders_by_category}
    title="Sales by Month, {inputs.category.label}"
    x=month
    y=sales_usd
    series=category
/>