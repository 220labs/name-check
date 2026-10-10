# name-check

Is a name free as a domain and as a handle, and what does the domain cost? One command, one
Markdown table, ready to paste into a naming doc.

```
$ ./name-check acme namecheckr --tlds io,dev,com
Probed 2026-10-10 02:50 UTC. `free` = the registry has no such name; not a registrar quote (…)

| name | .io | .dev | .com | GitHub | npm | PyPI |
| --- | --- | --- | --- | --- | --- | --- |
| acme | taken | taken | taken | taken | taken | taken |
| namecheckr | free | free | taken | taken | free | free |
| price: first / renewal / 10 y | $28.12 / $51.80 / $494 | $8.75 / $12.87 / $125 | $11.08 / $11.08 / $111 | | | |

Domains by .io whois, .dev RDAP, .com RDAP. Prices: Porkbun's public list, USD.
```

It is read-only: it buys, registers and reserves nothing.

## Run it

Python 3.9 or later, standard library only. No install, no keys.

```
./name-check NAME [NAME…] [--tlds io,ai,dev,com]
```

Each cell is `free`, `taken`, or `?` with a note under the table saying why it could not tell.
Exit 0 once the table is printed, 2 on a bad argument.

GitHub allows 60 unauthenticated lookups an hour. With `GH_TOKEN` or `GITHUB_TOKEN` set, or
the [GitHub CLI](https://cli.github.com/) logged in, it uses that token (5,000 an hour), sent
to api.github.com only.

## Where each answer comes from

- **Domains:** RDAP where IANA's bootstrap lists the TLD (404 free, 200 taken); otherwise whois,
  on the server IANA names for the TLD (.io and .co have no RDAP). One whois query per server at
  a time: some answer parallel connections with nothing.
- **GitHub:** `api.github.com/users/NAME`, which covers users and organisations alike.
- **npm:** `registry.npmjs.org/NAME`. **PyPI:** `pypi.org/simple/NAME/`, which also counts a
  project whose releases were all deleted (nobody can register that name).
- **Prices:** Porkbun's public price list (no account needed). Ten years is the first year plus
  nine renewals: the renewal is the real price, the first year often a loss-leader.

## What `free` does not promise

- A premium or reserved domain still reads `free`: the registry has no record of it, and the
  registrar charges more at checkout.
- .ai is often sold with a two-year minimum; the price list does not say so.
- npm and PyPI refuse a name too close to an existing one, and GitHub keeps some retired names,
  so a `free` handle is not always claimable.

## Tests

```
bash test.sh
```

A local stub answers as every registry, so the tests touch no network.

## License

MIT — see [LICENSE](LICENSE).
