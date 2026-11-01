# Contributing

**English** · [Español](CONTRIBUTING.es.md)

The wizard is open source (Apache License 2.0, see [LICENSE](LICENSE) and [NOTICE](NOTICE)) and anyone
can help improve it: open an [issue](https://github.com/europesip/hcl-dx-compose-labs-wizard/issues) or a
pull request.

- Keep it **one static page per file**, plain HTML, CSS and JavaScript: no frameworks, no build step, no
  dependencies and no network access (the Content-Security-Policy must stay `connect-src 'none'`). That is
  what lets anyone verify that credentials never leave the browser.
- Texts in **English and Spanish**.
- Keep the [NOTICE](NOTICE) file and the attribution to EUROPESIP as the original author.

**Where to change it.** The published copy lives in the labs repository
([`docs/`](https://github.com/europesip/hcl-dx-compose-labs/tree/main/docs)), served by GitHub Pages, and
both copies must stay identical. The `LAB TEMPLATES` block of `index.html` (the lab files used by
option C) is generated with `tools/sync-wizard-templates.py` in the labs repository: do not edit it by
hand. The full procedure is in the
[labs CONTRIBUTING.md](https://github.com/europesip/hcl-dx-compose-labs/blob/main/CONTRIBUTING.md#maintaining-the-configuration-wizard).
