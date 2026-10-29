# Free AI Coding Lab on Kaggle TPU

Run a capable open-weight coding model through a temporary API endpoint using Kaggle's free TPU quota—without buying a high-end GPU.

> **Important:** this is free-tier experimentation, not unlimited or guaranteed hosting. Kaggle quotas, verification requirements, session limits, availability, and provider policies apply. The endpoint is temporary and the model runs on remote infrastructure, not on your laptop.

## What this project helps you do

- Start a Qwen coding model on a Kaggle TPU notebook.
- Obtain a temporary API endpoint and authentication key.
- Connect a compatible coding agent or send a test request.
- Recover when a notebook session ends and the endpoint changes.
- Understand the privacy, reliability, quota, and account-policy trade-offs before relying on it.

## How it works

```text
Your laptop / coding agent
          |
          | HTTPS request + API key
          v
Temporary public tunnel URL
          |
          v
Kaggle notebook session
          |
          v
TPU v5e-8 (8 TPU chips) -> open-weight model
```

Your code and prompts are sent to the remote endpoint. This is **not local inference** and should not be used with confidential source code, credentials, customer data, or work covered by an NDA unless you have approval for that data flow.

## Quick start

### 1. Check Kaggle access

1. Sign in to [Kaggle](https://www.kaggle.com/).
2. Open account settings and complete any verification Kaggle requires for accelerator access.
3. Check the current accelerator quota and the terms that apply to your account. Quotas and eligibility can change.

### 2. Open the maintained TPU notebook

Use the Qwen TPU notebook maintained by the upstream implementation project:

**[Open Qwen on Kaggle TPU](https://www.kaggle.com/code/rahim3/qwen3-8-27b-bf16-on-kaggle-tpu-130-tok-s-api)**

Read the notebook's instructions before running it. Select the TPU accelerator specified by the notebook, enable Internet or dataset access only as instructed, and run the cells in order. Avoid editing cells you do not understand.

This repository is a setup guide and companion checklist; it does not repackage or claim ownership of the upstream model-serving implementation. See [Implementation and licenses](#implementation-and-licenses).

Kaggle, Cloudflare, and upstream model providers may change quotas, access, availability, pricing, or policies at any time without notice. A temporary endpoint is not a guaranteed service and should not be treated as permanent infrastructure.

### 3. Wait for the endpoint

When the notebook finishes startup, it should display an endpoint URL and an API key. Copy both into a local password manager or environment variables. Treat the URL and key as secrets. Do not commit them to GitHub, paste them into screenshots, or share them publicly.

### 4. Test the endpoint

The exact endpoint path and model identifier are provided by the running notebook. For an OpenAI-compatible endpoint, a typical check looks like this—replace the placeholders with the values printed by your session:

```bash
curl "$BASE_URL/v1/models" \
  -H "Authorization: Bearer $API_KEY"
```

If the notebook documents a different base path or authentication format, follow its instructions rather than this generic example.

### 5. Connect a compatible coding agent

The upstream project documents the exact configuration for supported agents. For an Anthropic-compatible client, the general pattern is:

```bash
export ANTHROPIC_BASE_URL="YOUR_SESSION_URL"
export ANTHROPIC_AUTH_TOKEN="YOUR_SESSION_API_KEY"
export ANTHROPIC_MODEL="MODEL_ID_FROM_NOTEBOOK"
```

Then start your coding agent using its documented command. Do not assume every client supports the same API protocol; use the configuration instructions for your specific agent.

Example environment-variable templates are in [`examples/agent.env.example`](examples/agent.env.example). They contain placeholders only.

## When the session stops

Kaggle sessions are temporary and subject to idle shutdown, accelerator quotas, and maximum session durations. The notebook or tunnel may stop; the public URL can change on a restart.

1. Return to the Kaggle notebook and check whether the session is still running.
2. If it has ended, start a new session and follow the notebook's startup steps again.
3. Copy the **new** endpoint URL and API key.
4. Update your local environment variables or agent configuration.
5. Re-run the endpoint health check.

Some setups may stop after a period of inactivity; the exact idle behavior is not guaranteed to be exactly 20 minutes. Do not rely on a keep-alive workaround to evade platform limits.

## Quotas and cost

- The service is not unlimited. Free accelerator hours are capped and can vary by account, region, demand, and current Kaggle policy.
- Startup time, downloads, compilation, and idle time consume some of the available session time.
- A temporary notebook endpoint is not equivalent to a production server with uptime guarantees.
- If you need predictable availability, compare the total time and effort with a metered hosted model API or an appropriately licensed dedicated machine.

## Account and acceptable-use risks

Using Kaggle as a temporary ML experimentation environment is different from turning it into a persistent general-purpose server. The platform's acceptable-use rules can change. Running a long-lived public inference service, attempting to bypass idle limits, or using compute outside permitted data-science/ML purposes may violate applicable rules and could lead to session termination or account restrictions. **An account ban is a risk, not a guaranteed outcome.** Read the current [Kaggle Terms](https://www.kaggle.com/terms) and [Kaggle policies](https://www.kaggle.com/policies) before use.

## Security checklist

- [ ] Use only data you are allowed to send to a remote service.
- [ ] Never expose an unauthenticated model endpoint.
- [ ] Keep the endpoint URL and API key private.
- [ ] Do not commit `.env` files, Kaggle credentials, tokens, or API keys.
- [ ] Do not disable security controls or use this for production workloads.
- [ ] Stop the session when finished, if the notebook provides a safe stop method.
- [ ] Review the model's license and the upstream project's license.

## Troubleshooting

| Symptom | What to check |
|---|---|
| TPU option unavailable | Account verification, current quota, and accelerator availability. |
| Notebook startup fails | Read the first failing cell's output and the notebook's current setup notes. |
| Endpoint URL stops responding | Check notebook status; restart the session if it has ended. |
| Agent authentication error | Confirm the current URL, key, protocol, and model ID. |
| Request times out | Try a shorter prompt, check endpoint health, and inspect the notebook logs. Long prompts can require substantial prefill time. |
| Model downloads are slow | Startup can be limited by download, dataset, or compilation time. Avoid repeatedly restarting without reading the error. |

## Repository structure

```text
free-ai-coding-lab-kaggle-tpu/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── session-recovery.md
│   └── security-and-policy.md
├── examples/
│   └── agent.env.example
└── .github/
    └── ISSUE_TEMPLATE/
        └── bug-report.md
```

## Implementation and licenses

The actual TPU model-serving notebook and engine are maintained in [ARahim3/kaggle-tpu-lab](https://github.com/ARahim3/kaggle-tpu-lab), with a [Qwen TPU notebook](https://www.kaggle.com/code/rahim3/qwen3-8-27b-bf16-on-kaggle-tpu-130-tok-s-api). This repository provides its own setup notes, operational checklist, and example configuration; it does not copy the upstream source code. Keep the upstream attribution and follow its license if you reuse or modify its code. Model weights have separate license terms.

## Contributing

Issues and pull requests that improve clarity, safety, compatibility, or troubleshooting are welcome. Please do not include secrets, API keys, private URLs, personal account information, or confidential prompt/code samples in issues.

## Disclaimer

This is a community setup guide, provided as-is. Accelerator access, performance, session behavior, model availability, and policy terms may change. No uptime, speed, account eligibility, or continued free access is guaranteed.
