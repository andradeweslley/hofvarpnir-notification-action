# Gná - Notification Action for GitHub Actions Workflow

GitHub Action that sends a notification to [Hófvarpnir](https://github.com/andradeweslley/notify-system) via `POST /api/notify`. Use it in any workflow to notify your Hófvarpnir instance (e.g. deploy started, success, or failed).

## Usage

### Only one run in the workflow

```yaml
- name: Notify deploy started
  uses: andradeweslley/gna@v1.0.0
  with:
    api_key: ${{ secrets.API_KEY }}
    title: "Deploy started"
    message: ${{ github.event.head_commit.message }}
```

### Always run in the workflow (to always notify in workflow completion)

```yaml
- name: Notify deploy result
  if: always()
  uses: andradeweslley/gna@v1.0.0
  with:
    api_key: ${{ secrets.API_KEY }}
    title: "Deploy finished"
    message: ${{ github.event.head_commit.message }}
    status: ${{ job.status == 'success' && 'success' || 'failed' }}
```

## Inputs

| Input     | Required | Default | Description |
|----------|----------|---------|-------------|
| `api_key` | Yes     | -       | Bearer token for `/api/notify`. Store in repo secrets (e.g. `API_KEY` or `NOTIFY_API_KEY`). |
| `title`   | Yes     | -       | Notification title. |
| `message` | No      | `""`    | Notification body (sent as `body` in the API). |
| `status`  | No      | `started` | One of: `started`, `success`, `failed`. |

## Secrets

- **`API_KEY`** (or `NOTIFY_API_KEY`): Your Hófvarpnir API key.

## License

MIT
