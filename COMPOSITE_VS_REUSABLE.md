# Composite Actions vs Reusable Workflows

## Key Differences

### Composite Action
- **Location**: `.github/actions/[action-name]/action.yml`
- **Used as**: A step within a job
- **Needs**: `actions/checkout@v4` to access local actions
- **Level**: Runs within an existing job
- **Use case**: Reuse a group of steps

### Reusable Workflow
- **Location**: `.github/workflows/[workflow-name].yml`
- **Used as**: An entire job
- **Needs**: `workflow_call` trigger
- **Level**: Runs as a separate job with its own runner
- **Use case**: Reuse entire jobs/workflows

## Examples in This Repo

### 1. Composite Action
**Definition**: `.github/actions/hello-composite/action.yml`
```yaml
runs:
  using: 'composite'
  steps:
    - name: Say hello
      shell: bash
      run: echo "Hello!"
```

**Usage**: `use-composite-action.yml`
```yaml
jobs:
  call-composite:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4  # Required!
      - uses: ./.github/actions/hello-composite
        with:
          who: "User"
```

### 2. Reusable Workflow
**Definition**: `reusable-hello.yml`
```yaml
on:
  workflow_call:
    inputs:
      who:
        type: string

jobs:
  greet:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello!"
```

**Usage**: `use-reusable-workflow.yml`
```yaml
jobs:
  call-reusable:
    uses: ./.github/workflows/reusable-hello.yml
    with:
      who: "User"
```

## When to Use Which?

**Use Composite Action when:**
- Reusing a set of steps within a job
- Want to keep steps together as a unit
- Need to share across multiple jobs in same workflow

**Use Reusable Workflow when:**
- Reusing entire jobs across workflows
- Need separate runner/environment
- Want to call from other repositories
- Need complex job-level features (matrix, services, etc.)

## Test Them

Run these workflows from Actions tab:
1. **Use Composite Action** - See action used as a step
2. **Use Reusable Workflow** - See workflow called as a job
