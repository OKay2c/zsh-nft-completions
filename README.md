# zsh completion for nft (nftables)

A feature-complete zsh completion script for [`nft`](https://netfilter.org/projects/nftables/), the nftables command-line tool.

> **Note:** This script was written by [Claude](https://claude.ai) (Anthropic's AI assistant).

## Features

- Completes all subcommands: `list`, `add`, `create`, `delete`, `flush`, `rename`, `insert`, `replace`, `get`, `reset`, `monitor`, `export`, `import`, `describe`
- Completes all global flags (`-j`, `-f`, `-n`, `-a`, `-t`, …) with descriptions
- Completes every object type per subcommand (`table`, `chain`, `rule`, `set`, `map`, `element`, `flowtable`, `counter`, `quota`, `limit`, `secmark`, `synproxy`, …)
- **Dynamic completions:** queries the running kernel via `nft list …` to offer your actual table, chain, set, and map names as you type
- Handles the optional `[family]` prefix (`ip`, `ip6`, `inet`, `arp`, `bridge`, `netdev`) correctly at every position
- `element` operations complete both set and map names, since either is valid

## Requirements

- Zsh
- `nft` (nftables ≥ 1.0 recommended)
- Read access to nftables for dynamic completions (typically `root` or `CAP_NET_ADMIN`); static keyword completion works without it

## Installation

### Manual

```zsh
mkdir -p ~/.zsh/completions
curl -o ~/.zsh/completions/_nft \
  https://raw.githubusercontent.com/OKay2c/zsh-nft-completions/main/_nft
```

Then add the following to your `~/.zshrc` **before** the `compinit` call:

```zsh
fpath=(~/.zsh/completions $fpath)
autoload -Uz compinit && compinit
```

Reload your shell:

```zsh
exec zsh
```

### Oh My Zsh

```zsh
cp _nft ~/.oh-my-zsh/completions/_nft
exec zsh
```

### System-wide

```zsh
sudo cp _nft /usr/share/zsh/site-functions/_nft
```

## Usage

Once installed, tab completion is available for the full `nft` command syntax:

```
nft list <TAB>                   # → ruleset, tables, table, chain, set, map, …
nft list table <TAB>             # → ip, ip6, inet, arp, bridge, netdev, <your tables>
nft list table ip <TAB>          # → tables in the ip family from your running ruleset
nft list chain ip filter <TAB>   # → chains in the ip/filter table
nft add <TAB>                    # → table, chain, rule, set, map, element, …
nft delete chain <TAB>           # → ip, ip6, inet, … and your existing table names
nft flush ruleset <TAB>          # → address families
nft monitor <TAB>                # → new, destroy, tables, chains, rules, …
nft -<TAB>                       # → all global flags with descriptions
```

## Notes

- Dynamic completion requires permission to call `nft list tables/table`. If run as a regular user without the necessary capabilities, dynamic name completion is silently skipped and only static keyword completion is offered.
- Tilde expansion works correctly in the `fpath` array form. If you prefer the environment variable form, use `FPATH="$HOME/.zsh/completions:$FPATH"` — a quoted `~` is **not** expanded by zsh.

## License

MIT
