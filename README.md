# Hófvarpnir Notification Action

GitHub Action that sends a notification to [Hófvarpnir](https://github.com/andradeweslley/notify-system) via `POST /api/notify`. Use it in any workflow to notify your Hófvarpnir instance (e.g. deploy started, success, or failed).

## Usage

### Only one run in the workflow

```yaml
- name: Notify deploy started
  uses: andradeweslley/hofvarpnir-notification-action@v1.0.0
  with:
    api_key: ${{ secrets.API_KEY }}
    title: "Deploy started"
    message: ${{ github.event.head_commit.message }}
```

### Always run in the workflow (to always notify in workflow completion)

```yaml
- name: Notify deploy result
  if: always()
  uses: andradeweslley/hofvarpnir-notification-action@v1.0.0
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
| `app_url` | No      | `https://hofvarpnir.weslleyandrade.dev.br` | Base URL of your Hófvarpnir instance. Override with a secret (e.g. `NOTIFY_APP_URL`) if you use a different URL. |

## Secrets

- **`API_KEY`** (or `NOTIFY_API_KEY`): Your Hófvarpnir API key (generate in your instance under Settings).
- **`NOTIFY_APP_URL`** (optional): Set if your instance is not at the default URL; pass as `app_url` in `with:`.

## License

MIT
