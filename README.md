# Peach AI Skills

Agent skills for integrating Peach products on Arc. Skills are organized by product capability, with explicit network boundaries. Each skill can be installed independently, including its `references/` directory.

| Product | Skill | Networks | Use it for |
| --- | --- | --- | --- |
| Aggregator | [arc-aggregator](skills/arc-aggregator/SKILL.md) | Arc Mainnet (`5042`) | Swap previews, Direct Allowance and Permit2 Witness integrations |
| Launchpad | [arc-launchpad](skills/arc-launchpad/SKILL.md) | Arc Testnet (`5042002`) and Mainnet (`5042`) | Token creation, curve trading, graduation and revenue claims |

These skills guide application development and review. They do not install a wallet, operate funds or provide a trading CLI or MCP server. SDKs are optional for direct HTTP or contract integrations.

## Install

From a clone of this repository:

```sh
npx skills add . --skill arc-aggregator --skill arc-launchpad
```

From GitHub:

```sh
npx skills add peachbuild/peach-ai-skills --skill arc-aggregator --skill arc-launchpad
```

Select only the skills you need. Alternatively, copy a complete skill directory into your agent's supported skills directory.

Example requests:

- "Use arc-aggregator to build an Arc Mainnet USDC swap preview."
- "Review our Arc Permit2 payer/executor flow and minimum-output checks."
- "Use arc-launchpad to implement Testnet token creation with an initial buy."

## Compatibility and sources

| Skill | Verified package | Source check |
| --- | --- | --- |
| `arc-aggregator` | `@masterpeach/arc-aggregator-sdk@1.0.0`, viem `2.55.10` | Preserved from the Arc skills release checked 2026-09-15 |
| `arc-launchpad` | `@masterpeach/launchpad-sdk@1.0.0`, viem `2.55.10` | Preserved from the Arc skills release checked 2026-09-15 |

The Arc skills retain the dated deployment references and verification limits from `peachbuild/peach-arc-skills` commit `a1aaa72`. Arc Aggregator request-mode and `v` guidance was clarified on 2026-10-23. See the [changelog](CHANGELOG.md) for the Arc release record.

Arc Aggregator uses viem and the `https://api.peach.ag/arc` API. Mainnet and Testnet use separate network and contract configurations. Deployment addresses are dated documentation snapshots, not proof of current on-chain wiring.

All skills preserve confirmed minimum output through refreshes and require explicit review of changed terms. SDK acceptance limits are not recommended risk limits. See the [behavioral acceptance cases](tests/scenarios.md).

## Validate

Requires Node.js 20 or newer. Structure checks have no dependencies:

```sh
node tests/check.mjs
```

For typechecking and offline checks, install the pinned packages in a temporary consumer with install scripts disabled:

```sh
consumer_dir=$(mktemp -d)
npm install --prefix "$consumer_dir" --ignore-scripts --no-audit --no-fund --save-exact \
  @masterpeach/arc-aggregator-sdk@1.0.0 \
  @masterpeach/launchpad-sdk@1.0.0 \
  viem@2.55.10 typescript@5.9.2
node tests/check.mjs --consumer "$consumer_dir"
```

The checker compiles the exact TypeScript examples and runs offline assertions. Behavioral cases also require review of the resulting integration. These checks do not establish production API compatibility, transaction simulation or on-chain execution. No approval, payer signature or transaction is sent by this repository's checks.

## License

[MIT](LICENSE) © 2026 giveuonepeach.
