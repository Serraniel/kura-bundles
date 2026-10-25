# kura-bundles

Curated data for [kura](https://github.com/Serraniel/kura): sets of apps
("bundles"), step-by-step fixes for known update problems ("flows"), hardware
rules, language-pack naming and repository profiles.

kura downloads this data, checks its signature and uses it. Nothing in here
is executed as a script.

## Folders

| Folder | Contents |
|---|---|
| `bundles/` | sets of packages and settings, for example for gaming |
| `flows/` | fixes for known problems that need specific steps |
| `hardware/` | which drivers and packages fit which hardware |
| `languages/` | how package families name their language packs |
| `providers/` | preferred package when several provide the same thing |
| `profiles/` | adding and removing other distributions' repositories |

The formats are described in [docs/format.md](docs/format.md). They are a
draft until kura milestone M6.

## Contributing

Pull requests are reviewed before merging, because kura applies this data on
users' computers. Please explain where a fix comes from (Arch news item,
forum thread, bug report) and on which system you tested it.

Sign off every commit (`git commit -s`, see the
[Developer Certificate of Origin](https://developercertificate.org/)) and use
Conventional Commit titles (`feat(flows): ...`).

## License

CC-BY-SA-4.0. See [LICENSE](LICENSE).
