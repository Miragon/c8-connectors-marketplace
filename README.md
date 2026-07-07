# Camunda 8 Connectors Marketplace

A community-maintained marketplace for the
[Miragon BPMN Modeler](https://github.com/Miragon/miranum-ide) that serves the
official element templates from
[`camunda/connectors`](https://github.com/camunda/connectors) — REST, Kafka,
SendGrid, Slack, and all the other out-of-the-box connectors.

This repository stores **no templates itself**. Its `marketplace.json` points
at a pinned release tag of `camunda/connectors`; the modeler fetches the
templates from there and keeps them up to date.

## Which branch should I use?

Connector element templates are tied to the connector runtime they invoke, so
this repository keeps **one branch per supported Camunda minor version**. Pick
the branch matching your cluster and register it in the modeler:

| Your Camunda version | Register this URL | Pinned connectors release |
| --- | --- | --- |
| 8.9 | `https://github.com/Miragon/c8-connectors-marketplace/tree/camunda-8.9` | `8.9.6` |
| 8.8 | `https://github.com/Miragon/c8-connectors-marketplace/tree/camunda-8.8` | `8.8.15` |
| 8.7 | `https://github.com/Miragon/c8-connectors-marketplace/tree/camunda-8.7` | `8.7.22` |

`main` always mirrors the newest supported minor (currently 8.9). Registering
`https://github.com/Miragon/c8-connectors-marketplace` without a branch gives
you `main` — fine if you always run the latest Camunda version, but note that
`main` rolls forward when a new minor is released.

Branches for minors that fall out of Camunda's support window stay around so
existing registrations keep working, but they no longer receive patch bumps.

## How to register

In the Miragon BPMN Modeler, add the branch URL from the table above as a
template marketplace (paste it into the *Add Marketplace* prompt, or add it to
the `marketplaces` setting). The modeler resolves the branch's
`marketplace.json` and merges the connector templates into the element-template
catalog.

## Contributing

The pins are maintained by the community — bump PRs are welcome:

- **Target the branch you want to bump.** A PR that raises the 8.8 pin goes
  against `camunda-8.8`; it must never change what 8.7 or 8.9 users receive.
- **Pin exact release tags** of `camunda/connectors` (e.g. `8.8.16`), never a
  branch or `latest` — registrations must stay reproducible.
- **Stable releases only** — no `-alpha` or `-rc` tags.
- When Camunda releases a new minor, open a PR against `main` that adds the
  new `camunda-8.x` branch row here and bumps the `main` pin; a maintainer
  cuts the branch on merge.

## Format

`marketplace.json` follows the Miragon marketplace format. Each `sources[]`
entry names a provider, repository, pinned `ref`, subtree `path`, and
`include` globs that select the element-template JSON files:

```json
{
    "sources": [
        {
            "provider": "github",
            "repo": "camunda/connectors",
            "ref": "8.9.6",
            "path": "connectors",
            "include": ["**/element-templates/*.json"]
        }
    ]
}
```
