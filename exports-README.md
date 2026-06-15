Reproducibility exports (kept as zip files in the repo root):
- agent-solution.zip               (Copilot Studio agent, exported as a Power Platform solution)
- PowerAutomate-Flow-trigger.zip   (Power Automate daily-digest scheduled flow export)

IMPORTANT: before committing, open each .zip and confirm it contains NO connection secrets,
tokens, client secrets, or credentials. Solution exports can include connection references —
remove any secret values. Synthetic data is fine; credentials are not.
