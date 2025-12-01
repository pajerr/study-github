# Hello Actions - GitHub Actions Study

A simple Docker-based GitHub Action for learning purposes.

## Structure

```
.
├── action.yml              # Action metadata
├── Dockerfile             # Docker container configuration
├── entrypoint.sh          # Script that runs inside the container
└── .github/
    └── workflows/
        └── hello.yml      # Workflow that uses the action
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

## For GitHub Actions Certification

This example demonstrates:
- Creating a Docker-based action
- Action metadata (inputs, branding)
- Workflow configuration
- Using actions in workflows
- Environment variable handling
