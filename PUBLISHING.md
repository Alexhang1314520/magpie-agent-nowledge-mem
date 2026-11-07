# Publishing and listing

The upstream publishing requirements are at https://usemagpie.ai/docs/zh/plugins.

## Standalone distribution

The source package is `magpie-agent-nowledge-mem`, with `package.json` at the repository root, all runtime files committed, and the GitHub topic `magpie-plugin`. It is installable directly from GitHub, including a version tag. It does not require an npm account for that route.

Before a release:

```sh
npm test
npm run check
MAGPIE_BIN=/path/to/magpie node tests/magpie-smoke.mjs
npm pack --dry-run
npm pack --pack-destination dist
```

The smoke test uses temporary Magpie settings, a copied Agent with a temporary selection path, disabled account discovery, and fake local servers. It never calls a paid model. The opt-in `tests/nowledge-contract.mjs` temporarily creates/removes a provider and refuses a backend with Remote mode enabled or configured providers; it is not a generic production test.

Publish GitHub release assets for the tested tag. The `magpie-plugin` topic makes the repository eligible for Magpie's **unreviewed GitHub discovery** channel; it is not an endorsement or community approval. The upstream docs describe a cache of up to ten minutes. The independent npm package can be published later from an authenticated npm account; never claim it exists on npm until the registry confirms it.

## Community contribution

Submit the Agent source under `packages/nowledge-mem-agent` to `magpie-community/plugins`, as `@magpie-community/agent-nowledge-mem`. The existing `packages/nowledge-mem` is the distinct gateway recall/capture plugin and must not be overwritten. The registry entry must use `kind: agent`, `community: true`, bilingual summaries, and no `providers` list. Repository metadata points to the community repository and the package directory.

Run the package's tests with both Node and Bun, then the community registry check:

```sh
node --test packages/nowledge-mem-agent/tests/*.test.mjs
bun test packages/nowledge-mem-agent/tests/*.test.mjs
bun scripts/check.mjs nowledge-mem-agent
```

Community review and merge belong to its maintainers. After merging, the upstream publish workflow distributes the package through its integrity-checked `dist` branch even before the npm organization performs a first npm publish. See https://github.com/magpie-community/plugins#publishing. A PR submission is pending review, not an approved listing.

Do not copy personal config, credentials, state journals, or local implementation notes into either public source package. Keep the first release's documented limitations and the uninstall-before-disconnect warning visible.
