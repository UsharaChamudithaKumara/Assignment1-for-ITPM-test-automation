# Test Automation - Singlish to Sinhala Translator

A Playwright-based test automation tool for validating the **Singlish to Sinhala translator** at [https://www.pixelssuite.com/chat-translator](https://www.pixelssuite.com/chat-translator).

## Overview

This tool automates testing of the chat translator by:
1. Reading test cases from an Excel workbook
2. Launching a browser and navigating to the translator UI
3. Entering Singlish input text for each test case
4. Capturing the actual Sinhala output
5. Comparing against expected output and recording PASS/FAIL status
6. Saving results back to the Excel file

## Prerequisites

- **Python 3.7+** installed
- **pip** package manager
- An Excel file with test cases (see [Excel Format](#excel-format) below)

## Installation

1. Navigate to the project directory:
   ```bash
   cd "d:\sllit 3rd year 2nd semester\ITPM\Assignment for test automation"
   ```

2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
   Or manually install:
   ```bash
   pip install playwright openpyxl
   ```

3. Install Chromium browser for Playwright:
   ```bash
   python -m playwright install chromium
   ```

## Usage

### Basic Run (Default Settings)

```bash
python test_automation.py
```

This will:
- Use `Assignment 1 - Test cases.xlsx` (default file)
- Auto-detect the header row
- Test against https://www.pixelssuite.com/chat-translator
- Save results to the same Excel file

### Custom Excel File

```bash
python test_automation.py --excel "path/to/your/testcases.xlsx"
```

### Run in Headless Mode (No Browser UI)

```bash
python test_automation.py --headless
```

### Custom Wait/Retry Settings

```bash
python test_automation.py --wait-ms 3000 --retries 5 --retry-wait-ms 500
```

- `--wait-ms`: Time to wait after input before reading output (default: 5000ms)
- `--retries`: Number of retry attempts if output is empty (default: 8)
- `--retry-wait-ms`: Time between retries (default: 1000ms)

### Save Progress Periodically

```bash
python test_automation.py --save-every 5
```

Saves the Excel file every 5 test cases (useful for long test runs).

### Custom Columns

If your Excel has different column names, specify them:

```bash
python test_automation.py --input-col "Singlish" --expected-col "Expected" --actual-col "Actual" --status-col "Result"
```

### Full Example

```bash
python test_automation.py \
  --excel "Assignment 1 - Test cases.xlsx" \
  --headless \
  --wait-ms 4000 \
  --retries 6 \
  --save-every 10
```

## Excel Format

Your test case file must contain:

### Required Columns
- **TC ID**: Test case identifier (e.g., `Neg_0001`)
- **Input**: Singlish text to translate
- **Expected output**: Expected Sinhala translation

### Auto-Generated Columns
- **Actual output**: Captured output from the translator (auto-filled)
- **Status**: Test result—PASS, FAIL, COLLECTED, or UI Error (auto-filled)

### Optional Columns
- **Input length type**: S (short), M (medium), L (long)
- **Singlish input types covered**: Category (e.g., Greetings, Commands)
- **Evidence or rationale**: Notes about the test case

### Example Rows

| TC ID    | Input length type | Input              | Expected output    | Actual output | Status | Singlish input types covered | Evidence or rationale             |
|----------|-------------------|--------------------|--------------------|-----------|----|---------|------|
| Neg_0001 | S                 | machan kohomada?   | මචං කොහොමද?       | (auto)    | (auto) | Greetings | Casual greeting and check-in. |
| Neg_0002 | M                 | suba udasanayak wewa | සුභ උදෑසනක් වේවා | (auto)    | (auto) | Greetings | Formal morning greeting.       |

## Test Case Files Included

### 1. Assignment 1 - Test cases.xlsx (Original)
- Mixed PASS/FAIL statuses from previous runs
- Use this to re-run and update with fresh results

### 2. Assignment 1 - Test cases - failed.xlsx
- All 50 test cases with Status = "FAIL"
- Use for testing failure scenarios or when you want a clean failure baseline

### 3. Assignment 1 - Test cases - updated.xlsx
- All 50 test cases with `Neg_XXXX` ID format (Neg_0001 to Neg_0050)
- Empty Actual output and Status columns
- Ready for a fresh test run

## Column Detection

The tool automatically detects your Excel columns by looking for common header names:

**Input Column** variants: "Input", "Singlish", "Text", "Source", "Sentence", "Test Input"

**Expected Output** variants: "Expected output", "Expected", "Expected Sinhala", "Sinhala"

**Actual Output** variants: "Actual output", "Actual"

**Status** variants: "Status", "Result", "Pass/Fail"

If auto-detection fails, use `--input-col`, `--expected-col`, `--actual-col`, `--status-col` to specify manually.

## Troubleshooting

### Error: File not found
- Ensure the Excel file path is correct
- Use absolute paths or run from the script directory

### Browser won't open / timeout errors
- Check internet connectivity
- The translator URL may be down; verify at https://www.pixelssuite.com/chat-translator
- Increase `--timeout-ms` (default 60000ms)

### "Could not find Chat UI locators" error
- The website HTML structure may have changed
- Try removing `--headless` to watch what happens in the browser
- Check the browser console for JavaScript errors

### Output not updating
- Increase `--wait-ms` to give the translator more time to respond
- Check if the input/output textareas are present on the page
- Try `--slow-mo-ms 100` to slow down all interactions for debugging

### Excel file locked / Permission denied
- Close the Excel file in Microsoft Excel or any other application
- Re-run the script

## Output Interpretation

- **PASS**: Actual output matches expected output exactly
- **FAIL**: Actual output differs from expected
- **COLLECTED**: No expected output was provided (data collection mode)
- **UI Error**: Browser/UI interaction failed (check console output)

## Tips for Test Case Design

1. **Keep inputs realistic**: Use actual Singlish phrases
2. **Vary length and complexity**: Include S (short), M (medium), L (long) inputs
3. **Test categories**: Greetings, Commands, Questions, Requests, etc.
4. **Document rationale**: Explain why each test case matters
5. **Start with COLLECTED**: Leave expected output blank, run once to collect outputs, then review and fill in expected values

## Performance Notes

- Average test time: ~500ms per case (depends on network)
- Full 50-case run: ~25–30 seconds (headless)
- Use `--save-every N` for large test suites (saves progress every N cases)
- Playwright auto-waits for DOM elements (usually fast)

## Contact / Support

For issues with the translator or test automation:
- Check the translator at https://www.pixelssuite.com/chat-translator
- Review this README's Troubleshooting section
- Check `test_automation.py` for detailed error messages in console output
