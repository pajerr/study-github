# GitHub Actions Matrix Build Strategy

## What is a Build Matrix?

A **matrix build** automatically creates multiple jobs by combining different values. Instead of writing separate jobs for each combination, you define the variables and GitHub creates all combinations automatically.

## How Matrix Works

### Simple Example:
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, ubuntu-20.04]
    node-version: [16.x, 18.x, 20.x]
```

**This creates 6 jobs automatically:**
1. ubuntu-latest + Node 16.x
2. ubuntu-latest + Node 18.x
3. ubuntu-latest + Node 20.x
4. ubuntu-20.04 + Node 16.x
5. ubuntu-20.04 + Node 18.x
6. ubuntu-20.04 + Node 20.x

## Math: How Many Jobs?

**Formula**: `jobs = os count × node-version count`
- 2 operating systems × 3 node versions = **6 jobs**

## Accessing Matrix Values

Use `${{ matrix.variable-name }}` to access the current combination:

```yaml
runs-on: ${{ matrix.os }}           # Uses the current OS
node-version: ${{ matrix.node-version }}  # Uses the current Node version
```

## Why Separate Build and Test Jobs?

### Build Job (in our example):
- Tests 2 OS × 3 Node versions = **6 combinations**
- Ensures code builds on all targets
- Creates more log output

### Test Job:
- Tests 2 OS × 2 Node versions = **4 combinations**
- Runs AFTER build completes (`needs: build`)
- Focuses on fewer, critical versions
- Easier to review test results

## Key Concepts

### 1. `strategy.matrix`
Defines the variables and their values to combine:
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, ubuntu-20.04]
    node-version: [18.x, 20.x]
```

### 2. `runs-on: ${{ matrix.os }}`
Each job runs on the OS from the current matrix combination.

### 3. `needs: build`
The `test` job waits for `build` job to complete before starting.

## Visual Flow

```
Build Job (6 jobs in parallel):
┌──────────────────────────────┐
│ ubuntu-latest + Node 16.x    │ ✓
├──────────────────────────────┤
│ ubuntu-latest + Node 18.x    │ ✓
├──────────────────────────────┤
│ ubuntu-latest + Node 20.x    │ ✓
├──────────────────────────────┤
│ ubuntu-20.04 + Node 16.x     │ ✓
├──────────────────────────────┤
│ ubuntu-20.04 + Node 18.x     │ ✓
├──────────────────────────────┤
│ ubuntu-20.04 + Node 20.x     │ ✓
└──────────────────────────────┘
         ↓ needs: build
Test Job (4 jobs in parallel):
┌──────────────────────────────┐
│ ubuntu-latest + Node 18.x    │ ✓
├──────────────────────────────┤
│ ubuntu-latest + Node 20.x    │ ✓
├──────────────────────────────┤
│ ubuntu-20.04 + Node 18.x     │ ✓
├──────────────────────────────┤
│ ubuntu-20.04 + Node 20.x     │ ✓
└──────────────────────────────┘
```

## Benefits

1. **DRY (Don't Repeat Yourself)**: Write job once, run many times
2. **Comprehensive Testing**: Test across multiple environments automatically
3. **Parallel Execution**: All combinations run simultaneously (faster)
4. **Easy Updates**: Add new version to array, auto-creates new jobs

## Common Use Cases

- **Multiple Node versions**: Test compatibility (14.x, 16.x, 18.x, 20.x)
- **Multiple OS**: Linux, macOS, Windows
- **Multiple databases**: PostgreSQL, MySQL, MongoDB
- **Multiple language versions**: Python 3.8, 3.9, 3.10, 3.11

## Example Output in GitHub UI

When you run the workflow, you'll see in Actions:
```
Build (ubuntu-latest, 16.x)
Build (ubuntu-latest, 18.x)
Build (ubuntu-latest, 20.x)
Build (ubuntu-20.04, 16.x)
Build (ubuntu-20.04, 18.x)
Build (ubuntu-20.04, 20.x)
Test (ubuntu-latest, 18.x)
Test (ubuntu-latest, 20.x)
Test (ubuntu-20.04, 18.x)
Test (ubuntu-20.04, 20.x)
```

Each one is a separate job with its own logs!

## Advanced: Include/Exclude

You can customize the matrix:

```yaml
strategy:
  matrix:
    os: [ubuntu-latest, ubuntu-20.04]
    node-version: [16.x, 18.x, 20.x]
    exclude:
      - os: ubuntu-20.04
        node-version: 16.x  # Skip this combination
    include:
      - os: ubuntu-22.04
        node-version: 20.x  # Add this extra combination
```

## For Certification

Key points to remember:
- Matrix creates **cross-product** of all values
- Use `${{ matrix.variable }}` to access current value
- `runs-on` can use matrix values
- Jobs run **in parallel** by default
- Use `needs` to create dependencies between jobs
