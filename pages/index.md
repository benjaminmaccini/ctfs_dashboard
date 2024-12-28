---
title: Central Texas Food System Dashboard
---

This is a proof of concept replication of the [Central Texas Food System Dashboard](https://centraltexasfoodsystem.org). Note this is not affiliated with the Central Texas Food Bank in any way, it is intended for learning purposes only. All copy outside this page is not my own and exists only for aesthetic parity.

### Dashboard Pages
- [Food Production](food_production/index)

## Why?

Why recreate something? Well, I am currently on a 2016 laptop with an old wifi card and a poor CPU. These restraints make online BI tools like Microsoft Power BI work poorly.

Also, I wanted to learn more about DuckDB, which is a really cool "query-anything" engine.

### Pros

- Code is easy to understand and edit for anyone, using tools that are transferable (Markdown, SQL)
- Hosting is cheap

### Cons

- Data lifecycle requires rebuilding the site (currently). There is support for other data sources as well -- but I need to look more into this.
- Evidence is a newer company and might not make it long term, this introduces risks. It is open source, so it is possible to fork and vendor dependencies

## Data

- Import from Excel
- Load into DuckDB