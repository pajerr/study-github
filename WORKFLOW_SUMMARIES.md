# GitHub Actions Workflow Summaries

## What is `$GITHUB_STEP_SUMMARY`?

A special environment variable that lets you create custom, formatted summaries for your workflow runs. The summary appears **separately** from the job logs in its own section on the workflow run page.

## Key Benefits

1. **Visibility**: Easy-to-read summary without scrolling through logs
2. **Persistence**: Stays on the workflow page permanently
3. **Markdown Support**: Full GitHub-flavored markdown formatting
4. **Multi-Step**: Any step can contribute to the summary

## How It Works

### Basic Usage

```yaml
- name: Create summary
  run: |
    echo "## My Summary" >> $GITHUB_STEP_SUMMARY
    echo "This is a summary!" >> $GITHUB_STEP_SUMMARY
```

### Important Notes

- Use `>>` (append) not `>` (overwrite)
- Each step appends to the same summary
- Summary persists after workflow completes
- Logs vs Summary: Logs show execution details, summary shows results

## What You Can Include

### 1. Headers
```bash
echo "## Main Header" >> $GITHUB_STEP_SUMMARY
echo "### Sub Header" >> $GITHUB_STEP_SUMMARY
```

### 2. Tables
```bash
echo "| Column 1 | Column 2 |" >> $GITHUB_STEP_SUMMARY
echo "|----------|----------|" >> $GITHUB_STEP_SUMMARY
echo "| Value 1  | Value 2  |" >> $GITHUB_STEP_SUMMARY
```

### 3. Lists
```bash
echo "- Item 1" >> $GITHUB_STEP_SUMMARY
echo "- Item 2" >> $GITHUB_STEP_SUMMARY
```

### 4. Links
```bash
echo "[Link text](https://example.com)" >> $GITHUB_STEP_SUMMARY
```

### 5. Code Blocks
```bash
echo '```json' >> $GITHUB_STEP_SUMMARY
echo '{"key": "value"}' >> $GITHUB_STEP_SUMMARY
echo '```' >> $GITHUB_STEP_SUMMARY
```

### 6. Emphasis
```bash
echo "**Bold text**" >> $GITHUB_STEP_SUMMARY
echo "_Italic text_" >> $GITHUB_STEP_SUMMARY
echo "`code`" >> $GITHUB_STEP_SUMMARY
```

### 7. Emojis
```bash
echo "✅ Success" >> $GITHUB_STEP_SUMMARY
echo "❌ Failed" >> $GITHUB_STEP_SUMMARY
echo "🚀 Deployed" >> $GITHUB_STEP_SUMMARY
```

## Real-World Examples

### Test Results Summary
```yaml
- name: Report test results
  run: |
    echo "## Test Results 🧪" >> $GITHUB_STEP_SUMMARY
    echo "| Status | Count |" >> $GITHUB_STEP_SUMMARY
    echo "|--------|-------|" >> $GITHUB_STEP_SUMMARY
    echo "| Passed | $PASSED |" >> $GITHUB_STEP_SUMMARY
    echo "| Failed | $FAILED |" >> $GITHUB_STEP_SUMMARY
```

### Deployment Summary
```yaml
- name: Deployment info
  run: |
    echo "## Deployment 🚀" >> $GITHUB_STEP_SUMMARY
    echo "- **Environment**: Production" >> $GITHUB_STEP_SUMMARY
    echo "- **URL**: [Visit Site](https://example.com)" >> $GITHUB_STEP_SUMMARY
    echo "- **Time**: $(date)" >> $GITHUB_STEP_SUMMARY
```

### Build Artifacts
```yaml
- name: Build summary
  run: |
    echo "## Build Output 📦" >> $GITHUB_STEP_SUMMARY
    echo "" >> $GITHUB_STEP_SUMMARY
    echo "Artifacts generated:" >> $GITHUB_STEP_SUMMARY
    echo "- \`app.zip\` (12.5 MB)" >> $GITHUB_STEP_SUMMARY
    echo "- \`docs.pdf\` (2.3 MB)" >> $GITHUB_STEP_SUMMARY
```

## Certification Tips

For the GitHub Actions certification, remember:

1. **Difference from logs**: Summaries are for **results**, logs are for **details**
2. **Markdown formatting**: Know how to create tables, lists, links
3. **Append operator**: Always use `>>` never `>`
4. **Multi-step contribution**: Each step can add to the summary
5. **Use case**: Best for test results, deployments, metrics, artifacts

## Examples in This Repo

- `manual-workflow.yml`: Basic parameter summary
- `advanced-summary.yml`: Complex multi-section summary with:
  - Build phase results
  - Test results table
  - Deployment information
  - Overall status
  - Code block with configuration

## Try It Yourself

1. Go to Actions tab
2. Run "Advanced Summary Example"
3. After completion, check the "Summary" section
4. Compare the clean summary vs the detailed logs
