# E-Commerce Sales & Delivery Analytics Dashboard (Power BI)

A four-page Power BI dashboard that analyses sales, product categories, sellers, payments and delivery performance for a Brazilian online marketplace. Built with Power Query, a relational data model and a dedicated DAX measures table.

![Executive page](<img width="1920" height="1020" alt="Executive" src="https://github.com/user-attachments/assets/fc496d6c-46db-4c15-9274-7d8b9b8cb5df" />
)

## Overview

The dashboard answers the questions a marketplace operations or sales team would ask:

- Which product categories and states drive the most business?
- Is the business growing over time?
- Which payment methods are used the most?
- Where do deliveries run late, and how do cancelled orders compare with delivered ones?
- How much does freight cost, and what drives it?

## Dataset

- **Source:** [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle).
- **Content:** about 99,000 anonymised orders from 2016 to 2018, with items, payments, reviews, customers, sellers, products and geolocation.
- **Note:** this is a public dataset, not company data. Values are in Brazilian reais (R$). Please see the dataset page for its licence terms. The raw CSV files are not included in this repository; download them from Kaggle.

## Dashboard pages

| Page | What it shows |
|---|---|
| **Executive** | Headline KPIs, revenue by category, customer and seller concentration by state, sales trend, and payment method breakdown |
| **Top & Bottom Performance** | Top and bottom 7 product categories, sales vs freight by state, and freight by payment type |
| **Seller** | Order count by seller and customer state, category performance table (sales, freight, review, installments) |
| **Delivery** | Average delivery time, delayed and cancelled orders, delivery time by state, and freight by category |

Screenshots: [Executive Page](<img width="1920" height="1020" alt="Executive" src="https://github.com/user-attachments/assets/04c61271-e85a-4f95-8a6d-30b4228b0c07" />
), [Top & Bottom Category](<img width="1920" height="1020" alt="Top and Bottom Performer" src="https://github.com/user-attachments/assets/5ec1f872-0ec4-4c12-9034-eff8ea703abd" />
), [Seller](<img width="1920" height="1020" alt="Seller" src="https://github.com/user-attachments/assets/8fcefc95-36cc-40bc-bf03-e58154526b10" />
), [Delivery](<img width="1920" height="1020" alt="Deliveries" src="https://github.com/user-attachments/assets/21e0c091-72b2-442e-8fe0-30a753dbeb41" />)
.

## Data model

Relational model with nine source tables plus a separate **DAX Measures** table:

`Order_dataset` (orders) connects to `Order_detail_dataset` (items, price, freight), `Payment_dataset`, `Review_dataset` and `Customer_dataset`. Items link to `Product_dataset`, `Product_Cate_dataset` (English category names) and `Seller_dataset`. `Geo-Location_dataset` links to customers and sellers by zip code prefix.

![Data model](<img width="1920" height="1020" alt="Data Model" src="https://github.com/user-attachments/assets/2f5997e5-70a0-4c3f-ba27-3bb4310646c8" />
)

## Key DAX measures

| Measure | Purpose |
|---|---|
| `Total Sales` | Sum of item prices (gross product value) |
| `Total Freight Cost` / `Avg Freight Cost` | Freight charged, total and per item |
| `Total Paid Amount (Incl. Freight)` | Item price plus freight |
| `Order Count` | Distinct orders |
| `Average Delivery Time (Day)` | Days from purchase to delivery for on-time orders |
| `Delayed Order Count` | Orders delivered after the estimated date |
| `Cancelled Orders` | Orders with a cancelled status |
| `Average Installment` / `Average Review` | Payment installments and review score |
| `Annual Sale` | Year-to-date sales for the trend visual |
| `Top / Bottom Performing Category` | Highest and lowest category by sales for the selected period |
| `KPI ... HTML` | Styled KPI cards built from HTML and DAX |

Examples:

```DAX
Order Count = DISTINCTCOUNT(Order_dataset[order_id])

Total Sales = SUMX(Order_detail_dataset, Order_detail_dataset[price])

Avg Freight Cost = AVERAGEX(Order_detail_dataset, Order_detail_dataset[freight_value])
```

## Key findings

- **Sales concentration:** Sao Paulo (SP) dominates, with roughly 40,000 customers and about 1,850 sellers, far ahead of the next states (RJ, MG).
- **Top categories:** Health_Beauty, Watches_Gifts and Bed_Bath_Table lead on sales.
- **Payments:** credit card accounts for about 78% of the paid amount, followed by boleto.
- **Freight:** average freight is about R$ 19.99 per item. Furniture_Mattress_And_Upholstery is the most expensive category at about R$ 42.91 per item.
- **Payment type and freight:** average freight per item varies by only about R$ 1.50 across payment types, so payment method has little visible effect on freight cost.
- **Delivery:** the average delivery time is 10.8 days. 6,535 orders arrived after the estimated date, and Sao Paulo has the most delayed orders by count while also having the shortest average delivery time (a volume effect). Bahia (BA) has the longest average delivery time.
- **Cancellations:** 625 orders were cancelled. Cancelled orders have an average delivery time of about 18.4 days, compared with 10.8 days for delivered orders. This is an association, not proof that slow delivery causes cancellations.
- **Trend:** monthly sales grew through 2017 and levelled off near R$ 1.0M a month in 2018. The drop at the right edge of the chart comes from an incomplete final period in the data.

## Limitations

- Public, anonymised data from a single marketplace, so findings may not generalise.
- The data ends in mid-2018, and the last month is incomplete.
- Orders can be paid with more than one payment method, so payment-type slices are based on item values and may not add up exactly to payment-table totals.
- Observations show relationships in the data. They do not prove causes.

## Tools

Power BI Desktop, DAX, Power Query.

## How to use

1. Download the dataset from Kaggle.
2. Open `E-Commerce.pbix` in Power BI Desktop.
3. Update the data source paths (Transform data, then Data source settings) to your local CSV location.
4. Refresh the model.

## Author

Arjun Singh
