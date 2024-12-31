---
title: Central Texas Food System Dashboard
---

This is a proof of concept replication of the [Central Texas Food System Dashboard](https://centraltexasfoodsystem.org). Note this is not affiliated with the Central Texas Food Bank in any way, it is intended for learning purposes only. All copy outside this page is not my own and exists only for aesthetic parity.

### Dashboard Pages

<Accordion>
    <AccordionItem title="Food Production">
        <LinkButton url="food_production">Learn More</LinkButton>
        <img src="/food_production.jpeg" />
    </AccordionItem>
    <AccordionItem title="Food Processing and Distribution">
        <LinkButton url="/food_processing">Learn More</LinkButton>
        <img src="food_processing.jpeg" />
    </AccordionItem>
    <AccordionItem title="Food Markets and Retail">
        <LinkButton url="/food_markets">Learn More</LinkButton>
        <img src="food_markets.jpeg" />
    </AccordionItem>
    <AccordionItem title="Food Consumption">
        <LinkButton url="/food_consumption">Learn More</LinkButton>
        <img src="food_consumption.jpeg" />
    </AccordionItem>
    <AccordionItem title="Food Waste">
        <LinkButton url="/food_waste">Learn More</LinkButton>
        <img src="food_waste.jpeg" />
    </AccordionItem>
</Accordion>


## Why?

Why recreate something? Well, I am currently on a 2016 laptop with an old wifi card and a poor CPU. These restraints make online BI tools like Microsoft Power BI work poorly.

Also, I wanted to learn more about DuckDB, which is a really cool "query-anything" engine.

### Pros

- Code is easy to understand and edit for anyone, using tools that are transferable (Markdown, SQL)
- Hosting is cheap
- Really fast to iterate functionality
- Transparency with regards to the data presentation
- Supports a large number of inputs/outputs reducing reliance on one solution
- Evidence provides a ton of built in tools for generating reports from the dashboard (export to PDF, downloading the data from the queries)

### Cons

- Data lifecycle requires rebuilding the site (currently). There is support for other data sources as well -- but I need to look more into this.
- Evidence is a newer company and might not make it long term, this introduces risks. It is open source, so it is possible to fork and vendor dependencies
- Presentation takes a bit more effort. The bare-bones app was quick to get running (less than 3 hrs), but adding custom branding/layout might be more work.

## Data Lifecycle

What is needed to update the data in the site? (Note that this could be improved to pull from remote sources)

- Pull latest codebase locally
- Import from Excel
- Load data into DuckDB
- Edit SQL functions (if necessary)
- Push code to Github

## Working Notes

- I noticed that although the data is sourced, it is quite difficult to reproduce the figures due to the lack of documentation surrounding the column values. For example, [withheld data](https://quickstats.nass.usda.gov/src/glossary.pdf) or how negative values are handled.
- Pie charts are bad. Switched to bar charts.
- There seems to be some instances of poor data presentation. For example, consumption data is only available as an aggregate value for 2016-2021 and 2017-2022 excluding the year 2012. When I change the year the value of consumption does not change, I would expect this to be available more granularly. Similarly, expense categories from the same plot exclude the aggregate averages for "other" food expenditures.
- Similarly, I could not easily track down saved data for the number of households per county per year to give the aggregate values in the original [Consumption vs Production Graph](https://www.centraltxfoodsystem.org/food-production#supply)
- Data organization in Excel neglects normalization standards.
- I really like how the data source and methodology is batched with the Excel files. It gives a PDF-like experience for data.