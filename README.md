# HCL DX Compose Labs · Configuration wizard

**English** · [Español](README.es.md)

Source code of the **optional configuration wizard** of the
[hcl-dx-compose-labs](https://github.com/europesip/hcl-dx-compose-labs) repository (HCL DX Compose
CF239 installed from scratch on Kubernetes, as a PoC).

- **Use it online (GitHub Pages):** start at the [about page](https://europesip.github.io/hcl-dx-compose-labs/about.html)
  or go straight to the [wizard](https://europesip.github.io/hcl-dx-compose-labs/).
- **Or run it locally:** download [`index.html`](index.html) (the wizard) and [`about.html`](about.html)
  (the explanations) and open them in your browser. They work offline.

| File | What it is |
|---|---|
| [`about.html`](about.html) | Presentation page: what the wizard does, the two output options, privacy and how to verify it. At the end: *continue to the wizard* or *follow the labs manually* |
| [`index.html`](index.html) | The wizard itself: the form, the generation of `env.sh` / `RUNBOOK.md` and the ZIP. All the logic is here |

> ⚠️ **Experimental · work in progress.** The wizard is evolving constantly and must be treated as
> experimental. It aims to make the installation easier and clearer, but **the recommended path
> today is the one without the wizard**: `cp env.example.sh env.sh`, edit it and follow the guides
> of each lab.

## What it does

It asks for your platform (Kubernetes, OpenShift or the WSL2 PoC), the DX host name, the
StorageClasses, namespaces and release names and the labs you want (every field pre-filled with the values of the
reference lab environment; only the Harbor and OpenRouter credentials are empty), and gives you three options:

| Option | You get | Then |
|---|---|---|
| **A · Only `env.sh`** | The configuration file the labs need | Follow the labs manually with their guides |
| **B · `env.sh` + `RUNBOOK.md`** (ZIP) | Also a personalised, **read-only** `RUNBOOK.md`: the instructions to build, one after the other, the labs you selected, with the scripts to run and the equivalent `kubectl` / `helm` commands using your values | Follow the runbook |
| **C · Direct installation** (ZIP with `dx-install/`) | A **self-contained, tailor-made installation**: every file (values, properties, LDIF, secrets) already filled in with your values, credentials in plain text; a `RUNBOOK.md` with short, easy-to-follow commands and an `install.sh` that runs them showing the lab and step. It does not need the labs repository (only `setup-postgres-dx.sql`, for Lab 2B, is copied in by hand) | `cd dx-install`, follow `RUNBOOK.md` or run `./install.sh` (`--list`, `--verify`, `--from <step>`) |

A and B go in the root of your clone of the labs repository and use the lab scripts. C is
independent and can be sent to someone who has nothing else; it carries credentials in plain text,
so keep it private and rotate them afterwards. The wizard itself never talks to your cluster.

To build option C, `index.html` carries a copy of the lab files (the `LAB TEMPLATES` block, plain
text that is never executed). In the labs repository, `tools/sync-wizard-templates.py` keeps that
copy identical to the lab files (`--check` reports differences).

## Why you can trust it with your credentials

The wizard is designed so that anyone can check that **credentials stay secret and are not stored
anywhere**:

- **One single file.** All the wizard logic is in [`index.html`](index.html) (`about.html` only holds text
  and the language switch): plain HTML, CSS and JavaScript,
  **no React, no frameworks, no build step and no dependencies**. What you read is exactly what
  runs, and it is easy to review. It is also why it works on GitHub Pages as is.
- **No network, enforced by the browser.** Both pages declare a Content-Security-Policy:
  `default-src 'none'; connect-src 'none'; script-src 'unsafe-inline'; ...` The browser blocks any
  request (`fetch`, XHR, WebSocket, external scripts, images or fonts): even if the code tried to
  send something, it could not.
- **Everything happens in your browser.** `env.sh` and `RUNBOOK.md` are generated in memory and
  downloaded with a local `blob:` link; the ZIP is built by a small function in the same file.
- **Secrets are optional.** Harbor token, OpenRouter key and passwords can be left empty and filled
  in later in `env.sh` (which is in the labs `.gitignore`: it is never committed).
- **Only one thing is stored**: the language you choose, in `localStorage`.

How to check it yourself:

1. Read `index.html` (the JavaScript is at the end, after the `LAB TEMPLATES` block).
2. Search it: there is no `fetch`, `XMLHttpRequest`, `WebSocket`, `sendBeacon` or external `src=`/`href=` to scripts or styles.
   The `LAB TEMPLATES` block is excluded because it is not code (text copies of the lab files for option C):
   ```bash
   sed '/BEGIN LAB TEMPLATES/,/END LAB TEMPLATES/d' index.html | grep -nE 'fetch\(|XMLHttpRequest|WebSocket|sendBeacon|<script src|<link '
   ```
3. Open the browser developer tools, **Network** tab, fill in the form and download: there are no requests.
4. Or run it locally with the network disconnected.

## About this wizard

It is a **simplified, functional option, specific to these labs**. EUROPESIP has another wizard
developed internally, with advanced features and more customisation options, available on demand:
https://www.europesip.com

The copy published on GitHub Pages lives in the labs repository
([docs/](https://github.com/europesip/hcl-dx-compose-labs/tree/main/docs)) and
are the same files as these.

## Licence and contributions

Open source under the [Apache License 2.0](LICENSE), originally developed by
[EUROPESIP](https://www.europesip.com): use it, modify it and redistribute it keeping the
[NOTICE](NOTICE) file. Contributions are welcome: [CONTRIBUTING.md](CONTRIBUTING.md).
