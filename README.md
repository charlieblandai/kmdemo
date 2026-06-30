# Konica Minolta U.S.A. — Bland Web Widget Demo

Same structure as `mywashington`:

- `index.html` — thin wrapper that loads the Bland widget and iframes the demo site
- `portal.html` — clone of the Konica Minolta U.S.A. homepage the widget floats over

## One step before it works: pathway → widget

The loader takes a **`widget_id`**, not a pathway id. Create a widget bound to the
pathway, then drop the returned id into `index.html`.

Pathway: `7182fe88-3e61-49f6-96e1-7142270b9efc`

**Option A — Dashboard:** app.bland.ai/dashboard/web-widget → New widget → select that
pathway → copy the widget id.

**Option B — API:**

```bash
curl -X POST https://api.bland.ai/v1/widget \
  -H "authorization: $BLAND_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "pathway_id": "7182fe88-3e61-49f6-96e1-7142270b9efc",
    "allowed_domains": ["localhost", "your-demo.vercel.app"],
    "messages_per_minute": 20,
    "config": { "timeoutSeconds": 3600 }
  }'
```

Use `data.id` from the response as the `widget_id` in `index.html`
(replace `PASTE_WIDGET_ID_HERE`).

`allowed_domains` must include wherever you host it, or the widget won't render.

## Deploy

Push both files to a repo and import to Vercel (static, no build), exactly like
`mywashington.vercel.app`. Then add the deployed domain to the widget's
`allowed_domains`.

## Optional tweaks (in `index.html`)

```js
window.blandSettings = {
  widget_id: "...",
  default_chat: true,                 // open straight into chat
  request_data: { source: "km_demo" } // becomes agent variables
};
```
