# E-Commerce Executive Sales Suite

A multi-page, interactive Power BI dashboard engineered to deliver actionable sales, customer, and product intelligence for executive leadership. This project transforms raw e-commerce transaction logs into a highly structured, corporate-ready analytical application built on a robust star schema data model.

## 📊 Dashboard Preview

<img width="659" height="370" alt="image" src="https://github.com/user-attachments/assets/936ac43a-2e23-45ca-b714-43dcf5515862" />


---

## 🚀 Key Technical Highlights
*   **Advanced Data Modeling:** Implemented a clean Star Schema data model with a custom chronological Calendar dimension spanning 2022-2025.
*   **Closed-Loop Architecture Resolution:** Resolved an ambiguous data path loop (`Transactions` -> `Customers` -> `Calendar`) using an **Inactive Relationship** triggered dynamically via specialized DAX functions.
*   **Dynamic Row Evaluation:** Utilized advanced context evaluation formulas (`RANKX` combined with `ISINSCOPE`) to generate real-time product rankings that dynamically recalculate based on active category slices.
*   **High-Density Tooltip Engine:** Engineered contextual, report-page tooltips including a synchronized table and categorical breakdown to surface deep-dive trends instantly upon hover without canvas clutter.

---

## 🛠️ Data Model Architecture
The dashboard relies on a centralized **Star Schema** to enforce structural integrity, maximize engine performance, and ensure strict calculation accuracy across all dimensions.

+-----------------------+
      |       Calendar        |
      +-----------------------+
          |               :
          | (1:*)         : (1:*) Inactive [SignupDate]
          v               v
+--------------+    +--------------+
| Transactions |<---|  Customers   |
+--------------+    +--------------+
      ^
      | (1:*)
+--------------+
|   Products   |
+--------------+

### Advanced Modeling Fix: Ambiguous Paths
Because `Calendar` filters `Transactions` directly, connecting `Calendar` to `Customers[SignupDate]` created a closed loop, generating an ambiguous filtering path error. 
*   **The Fix:** The `Calendar[Date] -> Customers[SignupDate]` relationship was set to **Inactive (Dashed Line)**.
*   **The Activation:** A dedicated DAX measure utilizes `USERELATIONSHIP` to explicitly force Power BI to activate this path *only* during the calculation of customer signups across the multi-year timeline (2022–2024), leaving the core transaction grain uncorrupted.

---

## 📐 Core DAX Architecture

Here are the critical business logic and performance measures written inside the `_Measures` table:

### 1. Dynamic Product Performance Ranking
Ensures that the ranking ignores individual row-level filters to evaluate the entire filtered product catalog comprehensively while gracefully leaving the grand total row blank.
```dax
Sales Rank = 
IF(
    ISINSCOPE(Products[ProductName]),
    RANKX(
        ALLSELECTED(Products[ProductName]), 
        [Total Sales], 
        , 
        DESC
    ),
    BLANK()
)
```

### 2. Time Intelligence (Month-over-Month Growth)
Calculates chronological trajectories across the active 2024 window without relying on hardcoded logic.
```dax
Sales PM = 
CALCULATE(
    [Total Sales], 
    DATEADD('Calendar'[Date], -1, MONTH)
)

MoM Growth % = 
DIVIDE(
    [Total Sales] - [Sales PM], 
    [Sales PM], 
    0
)
```

### 3. Inactive Path Activation (Customer Signups)

```dax
New Customers = 
CALCULATE(
    DISTINCTCOUNT(Customers[CustomerID]),
    USERELATIONSHIP(Customers[SignupDate], 'Calendar'[Date])
)
```

## 🖥️ Report Layout & User Experience
The dashboard spans three dedicated, high-impact pages structured with synchronized filtering logic and hidden tooltip layers:

### 1. Sales Overview
- **Objective:** Immediate health assessment of core business KPIs.

- **Layout:** High-level executive banner (Total Sales, Total Orders, Units Sold) linked to a chronological trend line (Current Month vs Prior Month) and horizontal category distribution.

- **Custom Tooltip:** Hovering over any trend point opens an interactive card showing exact metrics for Total Orders, Total Sales, and Sales PM inside a clean table layout alongside a Selling Categories horizontal micro-chart.

### 2. Customer Insights
- **Objective:** Analyze regional distribution, lifetime spending concentration, and acquisition trajectories.

- **Layout:** Tracks unique vs. new customer footprints over time via a smooth area gradient chart. Uses a Top N Filter to isolate the top 10 revenue-contributing customers to evaluate account concentration risk.

- **Dynamic Tooltip (Tooltip_DeepDive):** Hovering over a customer bar surfaces a localized profile detailing their individual total order volume and category spending split.

### 3. Product Performance
- **Objective:** Granular auditing of specific products and inventory velocity.

- **Layout:** Features an interactive category tile/dropdown canvas slicer linked to a high-density Hero Performance Matrix.

- **UX Indicators:** Integrates micro-data bars directly inside the grid for revenue visualization alongside directional conditional formatting icons (Green up-arrows / Red down-arrows) to spot MoM performance shifts instantly.

## 📈 Key Insights Uncovered
- **MoM Fluctuations:** The custom trend tooltip isolates exact monthly transaction dips down to the specific lagging category, letting supply chain teams address category-specific inventory gaps immediately.

- **Client Concentration:** The Top 10 Customer matrix quickly surfaces whether a massive portion of revenue is tied to a small group of buyers, helping mitigate risk.

- **Inventory Optimization:** Dynamically isolating Rank #1 items across specific categories helps separate high-volume/low-margin items from low-volume/high-margin premium products.

## 🛠️ Tools & Technologies Used
- **Power BI Desktop** — Canvas Design, Advanced Data Visualization, UI/UX Layout.

- **DAX (Data Analysis Expressions)** — Advanced Time Intelligence, Filter Context Overrides, Table Iterator Functions.

- **Power Query / M-Code** — Initial Dimension Typing and Star Schema shaping.
