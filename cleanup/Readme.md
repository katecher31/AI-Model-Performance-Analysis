# Data Cleaning Steps

Cleaning of `Log_Template.csv` for the AAI-500 Team 1 project. Decisions are based on `Log_Template.csv`, `Log_Template.xlsx` and the dataset `README_Raw.md`.

**Result:** 600 rows (all kept), 28 columns (no columns added). Durations are used exactly as logged.

## Steps

1. **Checked CSV against XLSX.** Both files hold the same 600 rows and 30 columns. The only differences are one cell in each HH:mm:ss.ms column. The CSV is the working file, and the row order is unchanged.
2. **Set aside 2 columns and renamed 28.** The two HH:mm:ss.ms columns are not used. The other 28 were renamed to snake_case.
3. **Fixed inconsistent text.**
   - Replaced en dashes with hyphens in 596 scenario names.
   - Corrected "viaTelegram" to "via Telegram" in 4 rows.
   - Removed stray quote marks from the workflow end time.
   - Trimmed extra spaces from the text columns.
4. **Merged input type labels.** "Image + Text" (8 rows) became "Text + Image". The README defines three input types and lists image + text input under Text+Image. The data does not record input order, so its effect cannot be tested.
5. **Labeled rows that never ran.** 128 blank success values became "Not executed". `success` now holds Yes (460), No (12) and Not executed (128). All rows are kept for success-rate analysis.
6. **Converted timestamps to dates.** The four timestamp columns are stored as UTC dates instead of text. No durations were recalculated.
7. **Ran checks (no changes).** Every completed row has a response time. Times range from 2 s to 246 s. Each model has 115 completed rows.

# Log_Template_Proposed_Clean - Data Dictionary

- **Rows:** 600
- **Columns:** 28 (original columns, renamed to snake_case and cleaned in place)
- **Files:** `Olena_Log_Template_Proposed_Clean.csv`, `Olena_Log_Template_Proposed_Clean.xlsx`

## Columns

| column | role | meaning |
|---|---|---|
| chat_id | identifier | Telegram chat; one constant value |
| test_id | design | Test number 1-18 |
| scenario_name | design | Scenario, dashes standardized |
| scenario_purpose | design | Purpose of the scenario |
| how_to_start | design / request characteristic | Trigger method |
| input_type | request characteristic | Text Only, Text + Image, Image Only, Mixed |
| model | main grouping | One of four models |
| image_input | reference | Image cell, shortened in the source |
| text_input | reference | Prompt or bot command |
| model_output | reference | Model answer |
| workflow_name | design | CSBI Bot 1 or Bot 2 |
| workflow_start_time | context | Workflow start, UTC date |
| workflow_end_time | context | Workflow end, UTC date |
| workflow_time_seconds | measure: workflow latency | As logged, whole seconds; running total in batch runs |
| model_start_time | context | Model call start, UTC date |
| model_end_time | context | Model call end, UTC date |
| model_time_seconds | measure: response time (main) | As logged, whole seconds |
| success | filter | Yes / No / Not executed |
| error_message | reference | Error text, if any |
| cpu_per_second_pct | measure: CPU (raw list) | One value per second |
| cpu_avg_pct | measure: CPU | Average CPU during the call |
| gpu_per_second_pct | measure: GPU (raw list) | One value per second |
| gpu_avg_pct | measure: GPU | Average GPU during the call |
| memory_per_second_mb | measure: memory (raw list) | One value per second |
| memory_avg_mb | measure: memory | Average memory during the call |
| throughput_per_minute | measure (mirrors response time) | About 60 / model_time_seconds |
| throughput_per_second | measure (mirrors response time) | Per-minute value / 60 |
| notes | reference | Tester notes |
