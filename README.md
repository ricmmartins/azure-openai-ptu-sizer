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

## 📄 License
[MIT](./LICENSE)
