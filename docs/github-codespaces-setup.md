# How To Set Up GitHub Codespaces

GitHub Codespaces provides a ready-to-use cloud environment for this training. Participants can start coding immediately without local installation.

## What's Inside

| Path | Description |
|------|-------------|
| `notebooks/` | Training notebooks used during sessions |
| `data/` | Example data for exercises |
| `.devcontainer/` | Codespaces container configuration |

---


## Before The Session: Avoid Unexpected Charges (2 minutes)

GitHub Codespaces is free up to a monthly allowance: 120 core-hours of compute and 15 GB-month of storage on a Free account; 180 core-hours and 20 GB-month on Pro. A typical training session on the default 2-core machine consumes a small fraction of this. The risk isn't the session itself, but a codespace left running, or simply left undeleted, after the training ends.

If you have a GitHub Pro account with a payment method on file, exceeding the free tier can result in charges. On a Free account (or any account without a payment method), GitHub blocks usage instead of charging.

To make sure this training costs you nothing, do these three things on your account *before* you create the codespace:

1. **Set a spending limit of $0.** Go to [github.com/settings/billing/spending_limit](https://github.com/settings/billing/spending_limit) and confirm Codespaces is set to **$0**. This is the default for new accounts but worth checking.
2. **Set the default idle timeout to 30 minutes.** Go to [github.com/settings/codespaces](https://github.com/settings/codespaces) → "Default idle timeout" → 30 minutes. The codespace auto-stops if you walk away.
3. **Set the default retention period to 7 days.** Same page → "Default retention period" → 7 days. Your codespace auto-deletes if you forget about it, freeing the storage quota.

After the session, you can also manually delete the codespace at [github.com/codespaces](https://github.com/codespaces) → **...** menu → **Delete**.

## Create A New Codespace

1. Open the repository page on GitHub.
2. Click the green Code button.
3. Open the Codespaces tab.
4. Click Create codespace on main.
5. Leave the default 2-core machine type selected.
6. Wait for the container to build and open in VS Code for the web. The first build takes a few minutes.

The environment is set up automatically: system libraries and Python packages are installed during the build, and the `pingkit` package is installed from source after the container starts. You do not need to run any setup commands yourself.

## Run Notebooks

1. Open the `notebooks/` folder in the file explorer.
2. Open the notebook assigned by the training lead.
3. Select the Python 3 kernel when prompted.
4. Run cells from top to bottom.

## Add More Packages

Add packages to [.devcontainer/Dockerfile](../.devcontainer/Dockerfile) in the `pip install` block, then rebuild the Codespace:

- Cmd/Ctrl + Shift + P
- Codespaces: Rebuild Container

## Common Troubleshooting

- First build takes too long: this is normal on initial startup; reload and reopen if needed.
- Package install fails: rerun the install commands and check internet/proxy settings.
- Kernel issues in notebooks: restart the kernel and rerun cells in order.
- Wrong repository version: verify your branch in the bottom-left status bar.
