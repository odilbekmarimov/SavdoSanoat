# Power BI Report Creation Guide with Drill-Through Functionality

This guide provides detailed instructions to create a Power BI report using the spreadsheet data containing the tables *Businessmen*, *Problem Categories*, *Employees*, *Problems*, and *Ministries*. The report will feature multiple pages with drill-through capabilities to view detailed information about each employee, businessman, business name, and problem category.

## Prerequisites

- **Power BI Desktop**: Ensure Power BI Desktop is installed. Download it from the [official Power BI website](https://powerbi.microsoft.com/desktop/) if needed.
- **Spreadsheet Data**: Use the provided data with the following tables:
  - *Businessmen*: `id`, `Full Name`, `Business Name`, `Region`
  - *Problem Categories*: `id`, `Category Name`, `Maximum Deadline (working days)`
  - *Employees*: `id`, `Name`, `Position`
  - *Problems*: `id`, `Businessman ID`, `Problem Category ID`, `Summary`, `Assignee`, `Is Finished`, `Satisfaction`, `Ministry ID`, and date fields
  - *Ministries*: `ID`, `Uzbek`, `English`, `info`

## Step 1: Import Data into Power BI

1. **Open Power BI Desktop**
2. **Get Data**:
   - Click **"Get Data"** > **"Text/CSV"**.
   - Save the data as `.csv` or `.xlsx`.
3. **Load the Data**:
   - Select your file and click **"Open"**.
4. **Transform Data**:
   - Use **Power Query Editor** to split data into tables.
   - Rename queries appropriately.
   - Remove unnecessary columns.
   - Click **"Close & Apply"**.

## Step 2: Set Up Relationships

1. **Switch to Model View**
2. **Create Relationships**:
   - Link `Problems[Businessman ID]` → `Businessmen[id]`.
   - Link `Problems[Problem Category ID]` → `Problem Categories[id]`.
   - Link `Problems[Assignee]` → `Employees[id]`.
   - Link `Problems[Ministry ID]` → `Ministries[ID]`.
3. **Verify Relationships**:
   - Ensure **one-to-many** cardinality.
   - Confirm cross-filter direction as **Single**.

## Step 3: Incorporate DAX in Power Query (If Applicable)

- If using measures, move them to **Data View**.
- Example:
  ```DAX
  Average Satisfaction = AVERAGEX(FILTER(Problems, Problems[Is Finished] = TRUE()), Problems[Satisfaction])
  ```

## Step 4: Create the Main Report Page

1. **Add a New Page**: Rename it **"Overview"**.
2. **Add Visuals**:
   - **Bar Chart**: Problems by category.
   - **Pie Chart**: Problems by ministry.
   - **Map**: Problems by region.
   - **Tables**: Employees, Businessmen, Business Names, Problem Categories.
3. **Format Visuals**

## Step 5: Set Up Drill-Through Pages

### Employee Details Page
1. **Add a Page**: Rename it **"Employee Details"**.
2. **Add Drill-Through Filter**: Drag `Employees[Name]` to the **"Drill-through"** field.
3. **Add Visuals**: Employee info, problems by category, problem details.

### Businessman Details Page
1. **Add a Page**: Rename it **"Businessman Details"**.
2. **Add Drill-Through Filter**: Drag `Businessmen[Full Name]`.
3. **Add Visuals**: Businessman info, problems by category, problem details.

### Business Name Details Page
1. **Add a Page**: Rename it **"Business Name Details"**.
2. **Add Drill-Through Filter**: Drag `Businessmen[Business Name]`.
3. **Add Visuals**: Business name info, problems by category, problem details.

### Problem Category Details Page
1. **Add a Page**: Rename it **"Problem Category Details"**.
2. **Add Drill-Through Filter**: Drag `Problem Categories[Category Name]`.
3. **Add Visuals**: Problem category info, problems by ministry, problem details.

## Step 6: Add DAX Measures

- Create new measures if moved from Power Query.
- Example:
  ```DAX
  Total Problems = COUNT(Problems[id])
  ```

## Step 7: Configure Drill-Through

1. **Enable Drill-Through**:
   - Right-click in tables and select **"Drill through"**.
   - Verify pages update correctly.
2. **Add Buttons** (optional).

## Step 8: Test the Report

1. **Check Drill-Through Functionality**.
2. **Validate Visuals**.

## Step 9: Publish the Report

1. **Save the Report**.
2. **Publish to Power BI Service**.
3. **Share and Set Permissions**.

## Additional Tips

- **Slicers**: Add region filters.
- **Date Analysis**: Create trend analysis pages.
- **Performance Optimization**: Keep visuals simple.

This guide ensures a complete Power BI report with drill-through functionality. Let me know if you need further refinements!
