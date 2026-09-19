# Retail Logistics Intelligence System

**Power BI dashboard that connects sales, inventory and supplier performance to show where a retailer loses money in its supply chain, and what to do about it.**

<!-- Replace with a real screenshot: upload to /images and link it -->
![Dashboard overview](images/dashboard-overview.png)

> **How to use this template:** everything in [square brackets] needs your real numbers or details. Delete any section you can't back up with your data. Never leave a bracket in the published version.

---

## 1. Business problem

[Who is the retailer? Real, anonymised or synthetic? One sentence.]

Management could not answer these questions without opening several separate files:

1. Which products are frequently out of stock, and what does that cost in lost sales?
2. Which suppliers deliver late or short, and how much does it affect operations?
3. Where is inventory tied up (overstock) while other lines run out?
4. Which regions or routes have the slowest or most expensive deliveries?

## 2. Key findings

> This is the most important section. Recruiters read this and skip the rest. Use numbers, e.g. "Supplier X accounts for 38% of late deliveries but only 12% of volume."

| # | Finding | Evidence | Recommended action |
|---|---------|----------|--------------------|
| 1 | [e.g. Two suppliers cause most late deliveries] | [X% of late orders, avg delay Y days] | [Renegotiate terms / add backup supplier] |
| 2 | [e.g. 15% of SKUs are overstocked] | [$/KES value tied up, days of stock] | [Reduce reorder quantities on these SKUs] |
| 3 | [e.g. Stockouts concentrate in one category] | [Lost sales estimate] | [Raise reorder point] |
| 4 | [Your finding] | [ ] | [ ] |

## 3. Dashboard pages

| Page | What it answers | Main visuals |
|------|-----------------|--------------|
| Executive Summary | How is the business doing overall? | KPI cards, monthly trend |
| Sales & Customers | [ ] | [ ] |
| Inventory | Stockouts vs overstock | [ ] |
| Supplier Performance | Who is reliable? | [ ] |
| Logistics & Delivery | On-time rate, delivery time, cost | [ ] |

<!-- Add one screenshot per page in /images -->

## 4. KPIs and how they are calculated

| KPI | Definition | DAX (edit to match your model) |
|-----|-----------|--------------------------------|
| On-Time Delivery % | Deliveries on/before promised date ÷ all deliveries | `DIVIDE(COUNTROWS(FILTER(Deliveries, Deliveries[DeliveredDate] <= Deliveries[PromisedDate])), COUNTROWS(Deliveries))` |
| Avg Delivery Lead Time | Average days from order to delivery | `AVERAGEX(Deliveries, DATEDIFF(Deliveries[OrderDate], Deliveries[DeliveredDate], DAY))` |
| Stockout Rate | SKUs at zero stock ÷ total SKUs | [ ] |
| Inventory Turnover | Cost of goods sold ÷ average inventory value | [ ] |
| Supplier Fill Rate | Units received ÷ units ordered | [ ] |

## 5. Data model

[Insert a screenshot of the Power BI Model view.]

- **Fact tables:** [Sales, Inventory, Deliveries...]
- **Dimension tables:** [Products, Customers, Suppliers, Date, Region...]
- **Relationships:** star schema, one-to-many, single direction [confirm].

## 6. Data and cleaning

- **Source:** [where the data came from, and whether it is real or synthetic]
- **Size:** [rows per table]
- **Cleaning done in Power Query:** [e.g. fixed date formats, removed duplicates, handled null supplier IDs, standardised product names]
- **Assumptions:** [e.g. "lost sales estimated as average daily sales × stockout days"]

## 7. Tools

Power BI (Power Query, DAX, data modelling) · Excel · [SQL / Python if used]

## 8. How to view

- **Open the file:** download `[filename].pbix` and open it in Power BI Desktop (free).
- **Live version:** [link, if you publish to the web]
- **Page-by-page screenshots:** see the `/images` folder.

## 9. What I would do next

- [e.g. Add a demand forecast so reorder points adjust by season]
- [e.g. Connect to live data with scheduled refresh]

## Author

**Thomas Oselu** · Data Analyst · Nairobi, Kenya
[LinkedIn](https://linkedin.com/in/thomas-oselu) · Thomasoselu1@gmail.com
