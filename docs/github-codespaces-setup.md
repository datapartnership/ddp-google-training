# How To Set Up GitHub Codespaces

GitHub Codespaces provides a ready-to-use cloud environment for this training. Participants can start coding immediately without local installation.

This guide has two parts:

- **[For participants](#for-participants)** — what you need to do to join the training.
- **[For trainers and maintainers](#for-trainers-and-maintainers)** — how to update the environment between sessions.

## What's Inside

| Path | Description |
|------|-------------|
| `notebooks/` | Training notebooks used during sessions |
| `data/` | Example data for exercises |
| `.devcontainer/` | Codespaces container configuration |

---

## For Participants

### Before The Session — Avoid Unexpected Charges (2 minutes)

GitHub Codespaces is **free up to a monthly allowance**: **120 core-hours** of compute and **15 GB-month** of storage on a Free account; **180 core-hours** and **20 GB-month** on Pro. A typical training session on the default 2-core machine consumes a small fraction of this. The risk isn't the session itself — it's a codespace left running, or simply left undeleted, after the training ends.

If you have a **GitHub Pro account with a payment method on file**, exceeding the free tier can result in charges. On a Free account (or any account without a payment method), GitHub blocks usage instead of charging.

To make sure this training costs you nothing, do these three things on your account *before* you create the codespace:

1. **Set a spending limit of $0.** Go to [github.com/settings/billing/spending_limit](https://github.com/settings/billing/spending_limit) and confirm Codespaces is set to **$0**. This is the default for new accounts but worth checking.
2. **Set the default idle timeout to 30 minutes.** Go to [github.com/settings/codespaces](https://github.com/settings/codespaces) → "Default idle timeout" → 30 minutes. The codespace auto-stops if you walk away.
3. **Set the default retention period to 7 days.** Same page → "Default retention period" → 7 days. Your codespace auto-deletes if you forget about it, freeing the storage quota.

After the session, you can also manually delete the codespace at [github.com/codespaces](https://github.com/codespaces) → **...** menu → **Delete**.

### Create A New Codespace

1. Open the repository page on GitHub.
2. Click the green **Code** button.
3. Open the **Codespaces** tab.
4. Click **Create codespace on main**.
5. Leave the default **2-core** machine type selected — it is sufficient for this training.
6. Wait for the container to build and open in VS Code for the web. The first build takes a few minutes.

The environment is set up automatically: system libraries and Python packages are installed during the build, and the `pingkit` package is installed from source after the container starts. You do not need to run any setup commands yourself.

### Run Notebooks

1. Open the `notebooks/` folder in the file explorer.
2. Open the notebook assigned by the training lead.
3. Select the **Python 3** kernel when prompted.
4. Run cells from top to bottom.

### Common Troubleshooting

- **First build takes too long.** This is normal on initial startup. If the page seems frozen, reload the browser tab.
- **Kernel issues in notebooks.** Restart the kernel (top of the notebook → Restart) and rerun cells in order.
- **A package import fails.** Tell the trainer — it likely needs to be added to the container configuration (see below).
- **Wrong repository version.** Check the branch name in the bottom-left status bar; it should read `main`.

---

## For Trainers And Maintainers

### How Dependencies Are Installed

The environment is built from two sources:

- **`.devcontainer/Dockerfile`** — installs system libraries (GDAL, GEOS, PROJ) and general Python packages (`pandas`, `altair`, `geopandas`, `ipykernel`) into the base image.
- **`.devcontainer/devcontainer.json`** — runs `pip install -e .` after the container starts, installing the `pingkit` package itself from `pyproject.toml`.

Use the right file for the right kind of change:

| You want to add… | Edit |
|---|---|
| A general-purpose Python library used in notebooks (e.g., `seaborn`) | `.devcontainer/Dockerfile` |
| A new dependency that `pingkit` itself imports | `pyproject.toml` |
| A system library (e.g., `libspatialindex-dev`) | `.devcontainer/Dockerfile` (apt block) |
| A VS Code extension for participants | `.devcontainer/devcontainer.json` (`extensions` list) |

### Adding A Package To The Dockerfile

Edit the `pip install` block in [`.devcontainer/Dockerfile`](../.devcontainer/Dockerfile):

```dockerfile
RUN pip install --no-cache-dir \
    pandas \
    altair \
    geopandas \
    ipykernel \
    your-new-package
```

Then rebuild the container:

- Cmd/Ctrl + Shift + P
- **Codespaces: Rebuild Container**

Commit and push the change so participants get the same environment on their next codespace.

### Recommendations Before A Training Session

- **Enable prebuilds** on the repository (Settings → Codespaces → Prebuilds) so participant codespaces start in seconds rather than minutes.
- **Test the full flow yourself** in a fresh codespace the day before — build time, notebook execution, and any data downloads.
- **Remind participants** to follow the cost-protection steps above. Most issues come from forgotten codespaces, not active use.

### Common Maintenance Issues

- **Build fails after editing Dockerfile.** Check the build log in the Codespaces panel; usually a typo or a package that needs an apt dependency.
- **`pip install -e .` fails on container start.** The `pyproject.toml` likely has a syntax issue or an unresolvable dependency. Run `pip install -e .` locally to reproduce.
- **Pinning versions.** This project does not currently pin versions in the Dockerfile. For long-lived training cohorts, consider pinning (e.g., `pandas==2.2.3`) so the environment stays reproducible across sessions.
