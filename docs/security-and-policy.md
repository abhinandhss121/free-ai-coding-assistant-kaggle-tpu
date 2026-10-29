# Security, privacy, and acceptable use

## Data handling

Prompts and source code sent through the tunnel reach remote infrastructure. Do not send secrets, private repositories, customer data, or NDA-protected material unless the relevant data owner explicitly permits it.

## Endpoint security

Treat both the tunnel URL and API key as credentials. Avoid posting them in public issues, screenshots, logs, shell history, or source control. Use a notebook implementation that provides authentication; do not expose an unauthenticated endpoint to the public Internet.

## Platform rules

Review the current [Kaggle Terms](https://www.kaggle.com/terms) and [Kaggle policies](https://www.kaggle.com/policies). A free notebook accelerator is intended for permitted workloads, not guaranteed persistent hosting. Attempts to bypass session limits or run disallowed server workloads may result in termination or account restrictions. An account ban is possible but not certain; no prediction about enforcement is made here.

## Operational limitations

- Free quota and eligibility can change.
- Sessions have time limits and may stop after inactivity.
- Public tunnel URLs are temporary and may change after restart.
- Performance depends on model, runtime version, prompt length, concurrency, and current accelerator availability.
- Do not use this as a production service or assume any service-level agreement.
