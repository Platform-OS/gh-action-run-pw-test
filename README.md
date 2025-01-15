# **Run Playwright Tests GitHub Action**

This composite GitHub Action provides a standardized way to **run Playwright tests**, **upload the test report**, and optionally execute custom pre-test and post-test commands. It also generates a summary of the test results with a link to the uploaded report.

## **Features**
- Runs Playwright tests with flexible configuration.
- Dynamically sets up directories for screenshots and reports.
- Allows pre-test (`before`) and post-test (`after`) commands.
- Uploads the test report and generates a URL for access.
- Supports skipping the test execution.
- Publishes a test report summary to the **GitHub Actions job summary**.

---

## **Inputs**

| **Input Name**  | **Required** | **Default** | **Description**                                                                 |
|------------------|-------------|-------------|---------------------------------------------------------------------------------|
| `before`         | No          | None        | Command(s) to execute **before** running the tests.                            |
| `after`          | No          | None        | Command(s) to execute **after** running the tests.                             |
| `skip`           | No          | None        | If set to `true`, the tests and related actions will be skipped.               |
| `npm-prefix`     | No          | `.`         | The prefix to the npm command, used for specifying the working directory.      |

---

## **Outputs**

| **Output Name**         | **Description**                                                |
|--------------------------|----------------------------------------------------------------|
| `TIMESTAMP`             | A unique timestamp used for creating directories and reports. |
| `REPORT_URL`            | The URL of the uploaded test report.                          |

---

## **Steps Performed**
1. **Setup Directory for Screenshots:**
   - Modifies `playwright.config.ts` to include a timestamped directory for screenshots.
   - Adds the timestamp to the GitHub Action output for later use.

2. **Execute Pre-Test Commands (Optional):**
   - Executes the custom `before` command(s) if provided and not skipped.

3. **Run Playwright Tests:**
   - Executes the Playwright test suite (`npm run pw-tests`) unless skipped.

4. **Execute Post-Test Commands (Optional):**
   - Executes the custom `after` command(s) if provided and not skipped.

5. **Publish the Report:**
   - Moves the Playwright report files to the appropriate directories.
   - Uploads the report and generates a public URL.
   - Outputs the report URL for further use.

6. **Generate Job Summary:**
   - Adds a summary to the GitHub Actions job log, including the test status and report URL.

7. **Skip Test Execution:**
   - If the `skip` input is set, the tests and related actions are skipped, and a message is logged.

---

## **Example Usage**

### **Basic Usage**
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Playwright Tests
        uses: ./path-to-action
        with:
          test-name: "E2E Tests"
```

### **Custom Pre- and Post-Test Commands**
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Playwright Tests
        uses: ./path-to-action
        with:
          before: "echo Running pre-test setup"
          after: "echo Cleaning up after tests"
```

### **Skipping Tests**
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run Playwright Tests
        uses: ./path-to-action
        with:
          skip: true
```

---

## **Generated Summary**
When the job completes, a summary is added to the **GitHub Actions job log**:

```markdown
### TEST REPORT
#### Status: success
#### Test report URL: https://example.com/path-to-report
```

---

## **Implementation Notes**
- The `playwright.config.ts` file is dynamically modified to include a unique `attachmentsBaseURL` for screenshots and reports.
- Uploaded reports are timestamped to ensure uniqueness.
- The `before` and `after` inputs allow custom commands to extend the Action’s functionality.

---

## **Contributing**
Contributions are welcome! Feel free to open issues or submit pull requests to enhance this composite action.


