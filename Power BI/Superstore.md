# 🛒 Superstore Sales Dashboard (Power BI)

## 📊 Project Overview
This interactive sales dashboard was built using Power BI to analyze historical performance from a fictional Superstore across categories, customer segments, geographic regions, and product lines. The purpose of this dashboard is to uncover actionable insights and demonstrate the ability to build end-to-end business intelligence solutions.

> **Dataset Summary:**  
> Based on a simulated sales dataset covering **4 years**, with over **3,000 orders**, **12,500 units sold**, and nearly **$734,000 in revenue**.

---

## Business Questions Answered
- Which **product categories and subcategories** are driving the most revenue?
- What **segments** (e.g. Consumer, Corporate) are the most profitable?
- Which **states, regions, and cities** contribute the most to net profit?
- Who are our **top customers** based on Customer Lifetime Value (CLV)?
- Which products offer the **highest profit margins** relative to sales?

---

## Key Insights
- **California** is the highest-grossing state with ~$146K in sales.
- **Furniture** lags behind Technology and Office Supplies in profitability despite decent sales.
- The **Consumer** segment contributes the most to revenue but has a lower average order value than Corporate.
- Several products have **strong sales but weak profit margins**, indicating a pricing or cost issue.
- Top customers like **Sean Miller** and **Tom Ashbrook** generate nearly $74K and $44k each respectively in lifetime value.

---

## Features & Techniques Used
- Power BI Measures and DAX (e.g. CLV calculation, profitability analysis)
- Interactive **drillthrough pages** by Category, Segment, Region, and State
- Customized **dynamic header text** that updates based on selected drillthrough field
- Conditional formatting, data labels, and tooltip enhancements
- Clean design with **sales vs. profit comparisons**, margin analysis, and customer-level breakdowns

---

## DAX Logic – Drillthrough Header

To enhance the user experience, I implemented a **dynamic drillthrough header** that displays the specific data point selected (e.g. "State (Texas)" or "Category (Furniture)"). This ensures clarity and context when users land on drillthrough pages.

```DAX
DrillthroughHeader = 
VAR SelectedCategory = SELECTEDVALUE('Superstore'[Category])
VAR SelectedSegment = SELECTEDVALUE('Superstore'[Segment])
VAR SelectedRegion = SELECTEDVALUE('Superstore'[Region])
VAR SelectedState = SELECTEDVALUE('Superstore'[State])

VAR IsCategoryFiltered = 
    ISFILTERED('Superstore'[Category]) && 
    CALCULATE(COUNTROWS(VALUES('Superstore'[Category]))) = 1

VAR IsSegmentFiltered = 
    ISFILTERED('Superstore'[Segment]) && 
    CALCULATE(COUNTROWS(VALUES('Superstore'[Segment]))) = 1

VAR IsRegionFiltered = 
    ISFILTERED('Superstore'[Region]) && 
    CALCULATE(COUNTROWS(VALUES('Superstore'[Region]))) = 1

VAR IsStateFiltered = 
    ISFILTERED('Superstore'[State]) && 
    CALCULATE(COUNTROWS(VALUES('Superstore'[State]))) = 1

RETURN
    "Viewing details for : " &
    SWITCH(
        TRUE(),
        IsCategoryFiltered, "Category (" & SelectedCategory & ")",
        IsSegmentFiltered, "Segment (" & SelectedSegment & ")",
        IsRegionFiltered, "Region (" & SelectedRegion & ")",
        IsStateFiltered, "State (" & SelectedState & ")",
        "No Filter Selected"
    )
