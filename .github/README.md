<!-- Managed from AloysJehwin/Jeffi_Stores (oss/templates); edits here are overwritten by the next export. -->
<p align="center"><a href="https://github.com/Jeffi-ERP"><img src="https://raw.githubusercontent.com/Jeffi-ERP/.github/main/assets/jeffi-erp-logo.svg" width="64" alt="Jeffi ERP"></a></p>

# workspace

Start here: shared root config (package.json, tsconfig, Vitest, ESLint), the DB schema loader, and oss/assemble.mjs, which clones every Jeffi ERP repo into place.

Part of [Jeffi ERP](https://github.com/Jeffi-ERP), where the Jeffi Stores commerce platform is developed in the open. In the workspace this repo is the workspace root. It builds and tests inside the assembled workspace, not on its own.

## Set up

```bash
git clone https://github.com/Jeffi-ERP/workspace.git jeffi-erp
cd jeffi-erp
node oss/assemble.mjs                  # clones every Jeffi ERP repo into its folder
ONNXRUNTIME_NODE_INSTALL=skip npm ci
```

Then follow [Set up the workspace](https://github.com/Jeffi-ERP/.github/blob/main/CONTRIBUTING.md#set-up-the-workspace) for the database, `.env.local` and a local admin. You never need Jeffi Stores' own keys. To change this repo, fork it and add your fork as a remote inside the workspace root.

## How a change gets in

1. Open a pull request against `main`. Nothing runs yet.
2. A maintainer reads it and adds the `ok-to-test` label. That runs **PR checks** and **Guard**. A new push removes the label until a maintainer looks again.
3. When both pass, a maintainer squash-merges it. The merge runs pre-build and build, then waits for a maintainer to approve the promotion.
4. After approval the change is opened as a pull request on [AloysJehwin/Jeffi_Stores](https://github.com/AloysJehwin/Jeffi_Stores) and ships with a later release. The export from that release brings it back here.

`.github/` and `LICENSE` (and `oss/` in workspace) come from AloysJehwin/Jeffi_Stores, and Guard rejects pull requests that change them. Open an issue for those instead.

## Checks

| Stage | Check | Command (from the workspace root) |
|---|---|---|
| PR checks (after `ok-to-test`) | Type check (everything) | `npx tsc --noEmit` |
| PR checks (after `ok-to-test`) | Admin API manifest | `npm run cli:check` |
| PR checks (after `ok-to-test`) | API test mirror | `node scripts/check-api-test-mirror.mjs` |
| Pre-build (after merge) | Type check (everything) | `npx tsc --noEmit` |
| Pre-build (after merge) | Admin API manifest | `npm run cli:check` |
| Build (after merge) | Unit tests · store-core | `npx vitest run --project store-core` |
| Build (after merge) | Build · controller | `npm run build:controller` |

Run a stage locally from the workspace root: `node oss/checks.mjs workspace pr`.

## Licence

[GNU Affero General Public License v3.0](../LICENSE) (`AGPL-3.0-only`). You may use, modify and redistribute this code, including in production. If you distribute a modified version, or run one as a service that people use over a network, you must make its source available to those users under the same licence. Jeffi Stores also offers a commercial licence for use without those conditions. Contributions are accepted on the terms in [CONTRIBUTING](https://github.com/Jeffi-ERP/.github/blob/main/CONTRIBUTING.md).
