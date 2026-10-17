# Grok Imagine 1.5 API (`grok-imagine-1.5-apimart`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-hero-perfume.jpg" width="820" alt="Sample render via the APIMart image API">
</p>

> **from $0.015 per image** — one OpenAI-compatible endpoint at `https://api.apimart.ai/v1`, no monthly plan required. *(observed 2026-10-12)*

**[Get an API key](https://go.apimart.ai/k-34584c)** · **[Live pricing](https://go.apimart.ai/k-397b40)** · **[Model page](https://go.apimart.ai/k-ee3d47)** · [⚡ 60-second quickstart](#quickstart)

**Why teams call Grok Imagine 1.5 (`grok-imagine-1.5-apimart`) through APIMart**

- **One key, entire catalog.** The same `https://api.apimart.ai/v1` base URL and `Authorization` header reach Grok Imagine 1.5 (`grok-imagine-1.5-apimart`) and 300+ other image, video and language models — switch the `model` field, not your client.
- **$1 minimum, pay as you go.** No subscription and no prepaid plan to size up front: top up from $1 and spend it on calls. There is no free quota to burn through first, so the price in this table is the price you pay.
- **The charge comes back in the response.** Every call reports the amount billed (`cost` / `credits_cost`), so a spend number is read per call instead of guessed at month end.
- **Async by design.** Submit, take the `task_id`, poll `GET /v1/tasks/{id}` — batching and retries are ordinary queue work, not a bespoke integration.

<!-- /conv-kit:v1 -->



Grok Imagine 1.5 is xAI's image generation route on APIMart, priced per image with edits included.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Per-image | `grok-imagine-1.5-apimart` | per delivered image, by resolution | see the [model page](https://go.apimart.ai/k-ee3d47) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/images/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Output | List price | Effective price |
| --- | --- | --- |
| default | 0.01875 | 0.015 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-397b40) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | what to generate, or what to change when editing |
| `size` | string | `auto` / `1:1` | aspect ratio, or `auto` where the route supports it |
| `resolution` | string | `1K` | output resolution tier — cost scales with it |
| `n` | integer | `1` | number of images per task, where the route allows more than one |
| `image_urls` | string[] | — | reference images as public HTTP(S) URLs or base64 data URLs |
| `nsfw_check` | boolean | `false` | runs `omni-moderation-latest` before submitting |

The table lists the fields accepted by the APIMart `images/generations` endpoint. Check the [model page](https://go.apimart.ai/k-ee3d47) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/images/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"grok-imagine-1.5-apimart","prompt":"A bamboo forest at sunrise, slow push-in","size":"1:1","resolution":"1K"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/images/generations", headers=HEADERS, timeout=60, json={
    "model": "grok-imagine-1.5-apimart", "prompt": "A bamboo forest at sunrise, slow push-in",
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["images"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**What is the Grok Imagine 1.5 model id?**

`grok-imagine-1.5-apimart`. See the route table above.

**How is it billed?**

**from $0.015 per image**. The completed task reports the exact amount in its `cost` field; a failed task is refunded.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-34584c) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [grok-imagine-1-5-prompts](https://github.com/igu3s5e5x90wx/grok-imagine-1-5-prompts)
- **Same model, other languages** — 10 more language builds: 中文 [grok-imagine-1-5-api-cn](https://github.com/ke6yg044xwihmo/grok-imagine-1-5-api-cn) · Deutsch [grok-imagine-1-5-api-de](https://github.com/je4w09hwo/grok-imagine-1-5-api-de) · Español [grok-imagine-1-5-api-es](https://github.com/hgi2nd3z1h8dn/grok-imagine-1-5-api-es) · Français [grok-imagine-1-5-api-fr](https://github.com/nn2bfsza/grok-imagine-1-5-api-fr) · Relay/gateway `grok-imagine-1-5-api-gateway` · Bahasa `grok-imagine-1-5-api-id` · 日本語 [grok-imagine-1-5-api-ja](https://github.com/vjuhp4dhedflc4/grok-imagine-1-5-api-ja) · 한국어 [grok-imagine-1-5-api-ko](https://github.com/zgfsenvosnw1/grok-imagine-1-5-api-ko) · Português [grok-imagine-1-5-api-pt](https://github.com/u2dec2i-ji/grok-imagine-1-5-api-pt) · Русский `grok-imagine-1-5-api-ru`
- **More models in this language** — [grok-image-api](https://github.com/hpcsd0apt/grok-image-api) · [grok-imagine-image-quality-api](https://github.com/xt5chw51l/grok-imagine-image-quality-api) · [qwen-image-2-0-pro-api](https://github.com/usen4/qwen-image-2-0-pro-api) · [seedream-4-0-api](https://github.com/qela34r8sb7/seedream-4-0-api) · [wan-2-7-image-pro-api](https://github.com/u3cdzay/wan-2-7-image-pro-api) · [z-image-api](https://github.com/t917b/z-image-api)
- **Topic hubs** — every build of this model, all languages: [grok-imagine-1-5](https://github.com/topics/grok-imagine-1-5) · [grok-imagine-1-5-api](https://github.com/topics/grok-imagine-1-5-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-ee3d47> | `apimart.ai/model/grok-image` |
| Live pricing | <https://go.apimart.ai/k-397b40> | `apimart.ai/pricing` |
| Get an API key | <https://go.apimart.ai/k-34584c> | `apimart.ai/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Grok Imagine 1.5 is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
