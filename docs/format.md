# Data formats (draft)

Status: draft for milestone M6 of kura. Field names may still change.

All files are YAML. Text shown to users is given per language; English is
required, other languages are optional and fall back to English.

kura never runs shell commands from these files. Everything is expressed
with the actions listed at the end. Each action can be shown in a preview and
undone.

## Common fields

```yaml
id: steam-gaming            # unique, lowercase, dashes
version: 1                  # bump on every change
name:
  en: Gaming with Steam
  de: Spielen mit Steam
description:
  en: Steam, the Proton compatibility layer and tools most games need.
```

## Bundles (`bundles/`)

A set of packages plus configuration.

```yaml
packages:
  required: [steam]
  recommended: [gamemode, lib32-gamemode, mangohud]
  optional:
    - name: proton-ge-custom-bin
      source: aur
      reason:
        en: Newer Proton with extra fixes for some games.
conditions:
  arch: [x86_64]
actions:
  - enable-repo: multilib
  - user-group: gamemode
```

## Flows (`flows/`)

Steps for a known problem that kura cannot solve with its built-in rules.

```yaml
match:
  error: unsatisfied-dependency     # typed libalpm error, never pacman text
  installed:
    lib32-audit: "<4.1.4-2"
steps:
  - remove: [lib32-audit, lib32-pam, lib32-libcap]
    keep: [audit, pam, libcap]
  - update: system
check:
  installed-not: [lib32-audit]
```

Flows may contain `ask` steps with choices. Each choice leads to its own list
of steps.

## Hardware rules (`hardware/`)

```yaml
match:
  pci:
    vendor: "10de"               # NVIDIA
    class: "0300"
    device-range: ["1e00", "ffff"]   # Turing and newer
suggest:
  packages: [nvidia-open-dkms, nvidia-utils, lib32-nvidia-utils]
  reason:
    en: Driver for your NVIDIA graphics card.
```

## Language-pack families (`languages/`)

How a package family names its language packs.

```yaml
family: tesseract-data
pattern: "tesseract-data-{iso639_2t}"
special:
  zh-Hans: tesseract-data-chi_sim
  zh-Hant: tesseract-data-chi_tra
```

Available placeholders: `{bcp47}` (de-DE), `{bcp47_lower}` (de-de),
`{iso639_1}` (de), `{iso639_2t}` (deu), `{iso639_2b}` (ger), `{posix}` (de_DE).

## Provider preferences (`providers/`)

```yaml
virtual: nodejs
prefer: [nodejs, nodejs-lts-jod]
```

## Repository profiles (`profiles/`)

Adding and removing another distribution's repositories, with the way back.
Structure to be defined in kura milestone M9.

## Actions

| Action | Scope | Undo |
|---|---|---|
| `install`, `remove`, `keep` | system | reverse transaction |
| `update: system` | system | generation |
| `enable-repo` | system | disable repo |
| `enable-service`, `enable-user-service` | system / user | disable |
| `user-group` | system | remove from group |
| `write-file` (from template in this repository) | system / user | restore previous file |
| `set-env` | user | remove entry |
| `ask` | — | — |

## Signing

CI signs `index.json` (list of files with SHA-256 hashes) on every merge to
main. kura downloads the index, checks the signature, then fetches files and
checks their hashes.
