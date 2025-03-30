
# Power BI Report Instructions: Business Problem Tracking Dashboard

## Data Model Setup

### Import Tables:
- `Businessmen`
- `Problem Categories`
- `Employees`
- `Problems`
- `Visits`
- `Ministries`

### Relationships:
- `Problems[Businessman ID]` → `Businessmen[id]`
- `Problems[Problem Category ID]` → `Problem Categories[id]`
- `Problems[Assignee]` → `Employees[id]`
- `Problems[Ministry ID]` → `Ministries[ID]`
- `Visits[Problem ID]` → `Problems[id]`

---

## DAX Measures (Create in `Problems` Table)
```dax
Total Problems = COUNTROWS(Problems)

Resolved Problems = CALCULATE([Total Problems], Problems[Status] = "Resolved")

Pending Problems = CALCULATE([Total Problems], Problems[Status] = "Pending")

Open Problems = CALCULATE([Total Problems], Problems[Status] = "Open")

Avg Satisfaction = AVERAGE(Problems[Satisfaction])

Avg Resolution Time (Days) = 
    AVERAGEX(
        FILTER(
            Problems,
            NOT(ISBLANK(Problems[When Finished]))
        ),
        DATEDIFF(Problems[Started Date], Problems[When Finished], DAY)
    )

Problem Resolution % = DIVIDE([Resolved Problems], [Total Problems], 0)

On-Time Resolution % = 
    VAR DaysTaken = DATEDIFF(Problems[Submission Date], Problems[When Finished], DAY)
    VAR MaxAllowed = RELATED('Problem Categories'[Maximum Deadline (working days)])
    RETURN
    DIVIDE(
        COUNTROWS(
            FILTER(
                Problems,
                DaysTaken <= MaxAllowed
            )
        ),
        [Resolved Problems]
    )
```

---

## Report Pages

### 1. Overview Dashboard

**Visual Elements:**
- **Top KPIs**: Total Problems, Resolved Problems, Pending Problems, Avg Satisfaction, Avg Resolution Time
- **Charts**:
  - Stacked Column: Problems by Category
  - Pie: Problem Status Breakdown
  - Table: Top 10 Ministries by Problem Volume
  - Map: Problems by Region (`Businessmen[Region]`)

**Drill-Through**: Enabled to categories, ministries, regions

---

### 2. Problem Category Details (Drill-through)

**Drill-through Filter**: `Problem Categories[Category Name]`

**Visuals:**
- KPIs: Total Problems, Avg Satisfaction
- Line Chart: Problems Over Time
- Bar Chart: Avg Resolution Time per Ministry
- Table: Problem Descriptions with Assignees

---

### 3. Business Details

**Drill-through Filter**: `Businessmen[Business Name]`

**Visuals:**
- KPIs: Total Problems, Avg Satisfaction
- Table: All Issues Faced by Business
- Column Chart: Problem Categories
- Table: Ministry & Employee Details

---

### 4. Businessman Profile

**Drill-through Filter**: `Businessmen[Full Name]`

**Visuals:**
- Card: Total Businesses
- Table: List of Businesses
- Bar Chart: Categories faced
- Table: Satisfaction & Resolutions

---

### 5. Ministry Dashboard

**Drill-through Filter**: `Ministries[Uzbek]`

**Visuals:**
- KPIs: Total Problems, Resolved %, On-Time %
- Column Chart: Problems by Category
- Matrix: Assignees from Ministry
- Table: Problem Summary and Status

---

### 6. Employee Insights

**Drill-through Filter**: `Employees[Name]`

**Visuals:**
- KPI: Total Assigned Cases
- Donut: Status Breakdown
- Table: Problem Summary
- Bar: Avg Satisfaction per Employee

---

### 7. Region Map View

**Visuals:**
- Map using `Businessmen[Region]`
- Size: Count of Problems
- Tooltip: Avg Satisfaction, Resolved %

---

## Layout Notes

Use button elements for navigation and enable "Back" on drill-through pages. Use consistent themes and blue-gray tones for corporate consistency.

Images for layout previews are provided separately.
