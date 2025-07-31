
# Azure OpenAI PTU Sizer

A practical toolkit to help Azure OpenAI users estimate Provisioned Throughput Units (PTUs), identify token usage patterns, and compare cost strategies.

## 🔍 Overview
This repo includes:

- 📊 **Azure Workbook** to visualize token usage and estimate PTUs
- 📈 **Excel-based PTU Calculator** for cost comparison and scenario modeling
- 🧩 **Workbook-to-Spreadsheet Mapping** to streamline the workflow
- 🧪 **Example workflow** to get you started in under 10 minutes

---

## 🚀 Getting Started

### 1. Clone or Download
```bash
git clone https://github.com/ricmmartins/azure-openai-ptu-sizer.git
cd azure-openai-ptu-sizer
```

### 2. Open the Workbook
Import the [Azure Workbook JSON](./workbook/ptu-usage-tracker-workbook.json) into your Azure Portal:

1. Go to **Azure Portal > Monitor > Workbooks**
2. Click **New > Advanced Editor**
3. Paste the contents of the workbook JSON
4. Update the data source with your **Log Analytics Workspace ID**

> ℹ️ No customization needed beyond selecting the right workspace.

### 3. Use the Spreadsheet
Open the [PTU_Sizing_Recommendation_Enhanced.xlsx](./spreadsheet/PTU_Sizing_Recommendation_Enhanced.xlsx) file. Update the highlighted **input cells only**:

- **B4**: PTU Price per 1M Tokens (Reserved)
- **B5**: Monthly Minutes (usually 43,800)
- **B6**: Average Tokens per Minute (from workbook summary)
- **B7**: PTU Conversion Rate (default: 50,000)
- **B8**: Desired PTUs for Fixed + Spillover Scenario

You can also edit PAYGO token prices for each model in the table below input cells.

---

## 🧩 Workbook → Spreadsheet Mapping

| Workbook Panel                           | Spreadsheet Field                                      | Notes                                                                 |
|------------------------------------------|--------------------------------------------------------|-----------------------------------------------------------------------|
| **Token Summary & PTU Recommendation**   | B6 – Average Tokens per Minute (Overall)               | Sum of Prompt + Completion tokens over time                          |
|                                          | B7 – PTU Conversion Rate                               | Default is 50,000 TPM per PTU                                        |
|                                          | B8 – Desired PTUs (for spillover scenario)             | You choose how much base PTU to reserve                              |
| **PTU Trend by Hour**                    | Supports peak PTU insights (shown in Excel’s Row 20+)  | Optional mapping; good for identifying spikes                        |
| **Rate Limit Errors Over Time**          | Context only – no direct cell mapping                  | Helps determine if you’re hitting caps on PAYGO                      |

---

## 🧪 Example Workflow

1. Run the Azure Workbook for the past **7 or 30 days**
2. Grab your average TPM from the **Token Summary** panel
3. Paste it into cell **B6** of the spreadsheet
4. Compare costs across full PTU, on-demand, and hybrid strategies

---

## 📊 Spreadsheet Breakdown

### Assumptions & Inputs

- **B4 – PTU Price per 1M Tokens (Reserved)**:
    - This is your negotiated or list price for PTUs.
    - You can find it in the [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) or your EA pricing sheet.
    - Default value used: **$3 per 1M tokens** — adjust if needed.

- **B5 – Monthly Minutes**:
    - Default is **43,800** (30 days × 24 hours × 60 minutes).
    - If modeling for 7 days, use `7 * 24 * 60 = 10,080`.

- **B6 – Average Tokens Per Minute**:
    - Get this from the Workbook: Token Summary panel shows total tokens used over time.
    - Use `Total Tokens ÷ Number of Minutes` to get the average TPM.

- **B7 – PTU Conversion Rate**:
    - Default is **50,000 tokens/minute per PTU**.

- **B8 – Desired PTUs for Fixed + Spillover**:
    - Choose a baseline PTU number to provision.
    - Useful if you want to combine fixed PTU with PAYGO for overflow.

### On-Demand Prices (Model-Specific)
- Edit the prices per 1M tokens for each model based on [Azure OpenAI pricing](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/openai-service/).

### PTU Calculation – Average Usage (Rows 13–18)
- Shows average tokens per minute and required PTUs for each model.
- Total tokens per model = average TPM × 43,800.
- PTUs = `ceil(total_tokens / 50,000)`.

### PTU Calculation – Peak Usage (Rows 20–24)
- Shows highest spike in tokens/min for each model.
- Use this to estimate buffer or detect overload risk.

### Scenario Analysis (Rows 27–31)
- Compares three strategies:
    - Full PTU only
    - PAYGO only
    - Fixed PTU + Spillover
- Helps find breakeven point based on your token consumption pattern.

---

## 📁 File Structure

```
azure-openai-ptu-sizer/
├── workbook/
│   └── ptu-usage-tracker-workbook.json
├── spreadsheet/
│   └── PTU_Sizing_Recommendation_Enhanced.xlsx
├── LICENSE
└── README.md (this file)
```

---

## 🛠️ Requirements
- Azure Subscription with OpenAI enabled
- Log Analytics workspace with `AzureDiagnostics` and `AzureMetrics`
- Microsoft Excel

---

## 🙌 Contributing
Pull requests are welcome! Let’s improve token observability and sizing for everyone.

---
