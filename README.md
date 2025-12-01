# Hello Actions - GitHub Actions Study

A simple Docker-based GitHub Action for learning purposes.

## Structure

```
.
├── action.yml                      # Action metadata
├── Dockerfile                      # Docker container configuration
├── entrypoint.sh                   # Script that runs inside the container
└── .github/
    └── workflows/
        ├── hello.yml               # Auto-trigger workflow (push/PR)
        ├── manual-workflow.yml     # Manual dispatch with multiple input types
        └── manual-hello.yml        # Manual dispatch using custom action
```

## How it works

1. **action.yml**: Defines the action metadata, inputs, and specifies it uses Docker
2. **Dockerfile**: Sets up an Alpine Linux container and copies the entrypoint script
3. **entrypoint.sh**: The actual script that runs when the action executes
4. **hello.yml**: A workflow that triggers the action on push/PR/manual dispatch

## Key Concepts

- **Docker Action**: Runs in a container, portable and consistent
- **Inputs**: GitHub Actions passes inputs as environment variables with `INPUT_` prefix
- **Workflow Triggers**: `push`, `pull_request`, and `workflow_dispatch` (manual run)

## Testing

1. Commit and push these files to your repository
2. Go to Actions tab in GitHub
3. You'll see the workflow run automatically, or click "Run workflow" to trigger manually

## Workflows Explained

### 1. hello.yml (Automatic Triggers)
- Triggers on: push, pull_request, workflow_dispatch
- Uses the custom Docker action
- Demonstrates automatic workflow execution

### 2. manual-workflow.yml (Manual Dispatch - Advanced)
- **Only** triggers manually via GitHub UI or API
- Demonstrates multiple input types:
  - `choice`: Dropdown selection (logLevel, environment)
  - `string`: Free text input (tags)
  - `boolean`: Checkbox (runTests)
- Shows conditional step execution based on inputs
- Creates workflow summary with job results

### 3. manual-hello.yml (Manual Dispatch - Custom Action)
- Combines manual dispatch with custom Docker action
- User provides inputs through GitHub UI
- Passes inputs to the custom action
- Demonstrates dynamic behavior based on user choices

## How to Run Manual Workflows

1. Go to your repository on GitHub
2. Click the "Actions" tab
3. Select the workflow you want to run (left sidebar)
4. Click "Run workflow" button (right side)
5. Fill in the input form that appears
6. Click "Run workflow" to execute

You can also trigger via GitHub CLI:
```bash
gh workflow run manual-workflow.yml -f logLevel=debug -f environment=production
```

Or via REST API:
```bash
curl -X POST \
  -H "Authorization: token YOUR_TOKEN" \
  -H "Accept: application/vnd.github.v3+json" \
  https://api.github.com/repos/pajerr/study-github/actions/workflows/manual-workflow.yml/dispatches \
  -d '{"ref":"main","inputs":{"logLevel":"debug","environment":"staging"}}'
```

## For GitHub Actions Certification

This example demonstrates:
- Creating a Docker-based action
- Action metadata (inputs, branding)
- Workflow configuration (automatic and manual triggers)
- Using actions in workflows
- Environment variable handling
- `workflow_dispatch` event with various input types
- Conditional step execution
- Workflow summaries
- Dynamic behavior based on user inputs
