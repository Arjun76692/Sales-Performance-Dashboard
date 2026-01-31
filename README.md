# Sales Performance Dashboard (Power BI)

## What This Is

An interactive Power BI dashboard that lets you explore sales data across regions, categories, and products. Built it to practice DAX and understand how drill-down navigation works in BI tools.

Instead of looking at flat Excel tables, you can click through from "West region is underperforming" → "Which categories?" → "Which specific products?" in seconds.

## Dataset

**Source**: Superstore Sales (Kaggle)  
**Size**: 9,994 transactions  
**Scope**: 4 years of sales data across:
- 4 regions (East, West, Central, South)
- 3 categories (Technology, Furniture, Office Supplies)
- 17 sub-categories
- 1,800+ unique products

## What's in the Dashboard

### Page 1: Executive Overview
- Total revenue, profit, units sold (KPI cards)
- YoY revenue growth %
- Profit margin % by quarter
- Regional performance heatmap

### Page 2: Category Deep-Dive
- Revenue by category and sub-category
- Profit margin comparison across categories
- Product performance table (top 20 by profit)

### Page 3: Regional Variance Analysis
- Region-wise revenue trends (line charts)
- Variance from average (is this region over/underperforming?)
- Filters: Date range, category, customer segment

## Key DAX Measures I Built

**YoY Revenue Growth**:
```DAX
YoY Growth % = 
VAR CurrentYearRevenue = SUM(Sales[Revenue])
VAR PreviousYearRevenue = 
    CALCULATE(
        SUM(Sales[Revenue]),
        DATEADD(Sales[OrderDate], -1, YEAR)
    )
RETURN
    DIVIDE(
        CurrentYearRevenue - PreviousYearRevenue,
        PreviousYearRevenue,
        0
    ) * 100
```

**Profit Margin %**:
```DAX
Profit Margin % = 
    DIVIDE(
        SUM(Sales[Profit]),
        SUM(Sales[Revenue]),
        0
    ) * 100
```

**Regional Variance**:
```DAX
Variance from Avg = 
VAR RegionRevenue = SUM(Sales[Revenue])
VAR AvgRevenue = 
    CALCULATE(
        AVERAGE(Sales[Revenue]),
        ALL(Sales[Region])
    )
RETURN
    RegionRevenue - AvgRevenue
```

## Drill-Down Flow

The dashboard lets you navigate:

1. **Region level**: "West region: $500K revenue"
2. **Category level**: "Technology: $250K, Furniture: $150K, Office: $100K"
3. **Product level**: "Chairs: $80K, Tables: $70K..."

Each level shows profit margins, YoY growth, and volume metrics.

## Insights I Found

- **Technology** has highest revenue but **Furniture** has best profit margins (18% vs 12%)
- **West region** underperforms by 15% compared to national average
- **Phones** category has negative profit margin (-8%) due to heavy discounting
- Q4 shows 25% revenue spike (holiday season effect)

## Tech Details

**Tool**: Power BI Desktop  
**Data Model**: Star schema (Fact: Sales, Dimensions: Products, Time, Regions)  
**Relationships**: Auto-detected + manual date table  
**File Size**: ~2MB (.pbix file)

## What I Learned

**Power BI Specifics**:
- DAX is similar to Excel formulas but context matters a lot
- CALCULATE is incredibly powerful for filtering
- Date tables are essential for time intelligence
- Relationships need to be clean (avoid many-to-many)

**Dashboard Design**:
- Less is more (don't put 20 charts on one page)
- Colors matter (green for profit, red for loss)
- Drill-down > separate pages
- Filters should be intuitive (slicers, not dropdowns)

**Business Context**:
- YoY growth is better metric than absolute numbers
- Margins matter more than revenue (selling $1M at -10% margin is bad)
- Variance analysis helps spot outliers

## How to Use

1. Download the .pbix file
2. Open in Power BI Desktop (free version works)
3. Click around - it's interactive
4. Use slicers to filter by date, region, category

## Limitations

- Dataset is 4 years old (2014-2017)
- US market only (not global)
- No customer demographics (age, gender, etc.)
- Missing discount strategy data (why are phones discounted so much?)

## Future Enhancements

- **Forecasting**: Use Power BI's AI features to predict next quarter
- **Cohort analysis**: When did customers who bought in 2014 churn?
- **Product recommendations**: "Customers who bought X also bought Y"
- **What-if parameters**: "If we increase furniture prices by 5%, what happens to margin?"
- **Mobile layout**: Currently desktop-only
- **Automated refresh**: Connect to live database instead of static CSV
- **Alerts**: Notify when profit margin drops below threshold

## Why This Matters

This is basically what every company needs:
- **Sales team**: Which regions need attention?
- **Product team**: Which products are underperforming?
- **Finance team**: Are we hitting margin targets?

The drill-down capability is what makes it useful - you can start broad ("revenue is down 10%") and dig into root cause ("oh, it's just West region phones").

Same logic applies to product analytics:
- Start: "DAU is down 15%"
- Drill: "Which cohort? → Android users"
- Drill: "Which version? → 12.3.1"
- Root cause: "Bug in onboarding for Android 12.3.1"

---

**Tool**: Power BI Desktop  
**Dataset**: Superstore Sales (Kaggle)  
**Transactions**: 9,994  
**Date**: November 2025  
**File**: chocolate-analysis-vague.pbix (yeah, I named it badly - it's actually sales data, not chocolate)
