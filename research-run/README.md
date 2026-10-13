# Research Runner / Research Run 1.0.1

A portable instruction workflow for evidence-based research. It clarifies the
question and budget, researches in reviewed rounds, keeps evidence and continuation
records, and produces a self-contained HTML report. This repository distributes
the owner's workflow; it is not an executable research service.

[Download the ZIP](https://github.com/josiahH-cf/research-runner/releases/download/v1.0.1/research-run-1.0.1.zip)
or browse the [workflow](SKILL.md).

## Start with the files

1. Unzip the release and keep the complete `research-run/` folder together.
2. Make that folder available through your agent host's supported skill upload,
   registration or file-loading route. Have the agent read `SKILL.md` first.
   Native triggers work only after the host actually exposes the skill.
3. Supply a writable `research-runs` folder in your request. Optionally copy
   `config.example.json` to `config.json`, fill in your absolute folder paths,
   and omit `runs_root_wsl` if it does not apply. Keep local configuration private.
4. Invoke `research-run <topic>` or `research-run <existing run>`. Choose a token
   cap, clarify the scope and approve the frame before research begins.

Synthetic prompt example, not an executed run:
`research-run How could a fictional town library compare two e-book lending policies?`

## Requirements and limits

The host needs instruction/file access and permission to write the runs folder.
External research needs working search and page-access tools. Delegation is
optional; sequential roles and self-review must be disclosed when it is absent.
There is no controller, provider SDK or package installation in this release.
Native app activation is host-specific, and cloud access to local files is not
assumed. The generated HTML uses no JavaScript or external assets.

The cap covers coordination, delegated work, research, review and the report.
Usage is measured when available, otherwise conservatively estimated and labeled.
The instructions protect a report reserve and direct research to stop before
spending it. This is budget accounting and stop guidance, not an enforced token
or monetary ceiling; agent compliance and tool capability still matter.

## Provenance, privacy and license

This is a distribution snapshot of Research Run 1.0.1. Maintenance remains with
the original owner and private upstream; no installation is moved or changed.
See `PROVENANCE.md` for source hashes. The configuration example is portable.
No real configuration, registry, runs, supplied material, reports, credentials
or machine paths are included. Review real records and report contents separately
before sharing them; they may preserve exact requests, excerpts and identities.

MIT licensed. Retain the copyright and permission notice in `LICENSE` when copying
or distributing the workflow.
