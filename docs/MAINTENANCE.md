# Public documentation maintenance

[Home](../README.md) · [Status](BUILD_STATUS.md) · [Development log](DEVELOPMENT_LOG.md)

## Scope

I maintain my public presentation in a separate workspace from the technical project. I use Codex to help with documentation and Claude Code to help with technical development. For a documentation update, the technical project must remain read-only: do not switch its branches, modify its files, compile it, stop processes or run its scripts.

I review and publish updates manually. I have not enabled scheduled publication, filesystem synchronization or any documentation workflow with write access to my development directory.

## Review procedure

1. Read the current development instructions, progress notes, test results and architecture decisions. Note the review date and inspected development revision privately. Source files may contain newer uncommitted work.
2. Inspect existing completed logs and relevant source. Do not upload those files to a service. Confirm each proposed claim against its evidence. Treat partial or changing logs as unverified.
3. Separate generated code, compiled objects, executed paths, validated comparisons and playability. Record unresolved failures and differences. Do not carry a historical result forward to a new generation without evidence.
4. Add a development log entry only for a meaningful verified milestone. Update build status and the roadmap where necessary. Keep README.md and README.fr.md aligned in numbers, limitations and targets.
5. Review the exact publication diff. Stage only the explicitly approved documentation paths listed below. Never use bulk directory copying, `git add .` or `git add -A` from a directory containing development files.
6. Check Markdown structure, navigation, translation parity and the staged file inventory. Exclude local evidence notes, game files, binary data, keys, secrets and personal paths. Check repository visibility and destination before publishing.
7. Publish only the reviewed documentation commit when authorized. Re-read the remote files and repository metadata afterward. Do not create a game release.

## Publication allowlist

- README.md
- README.fr.md
- docs/ARCHITECTURE.md
- docs/ROADMAP.md
- docs/DEVELOPMENT_LOG.md
- docs/BUILD_STATUS.md
- docs/FAQ.md
- docs/MAINTENANCE.md

A future session should recheck access and remote contents before resuming a blocked publication. Do not overwrite existing work. No source publication is authorized by this procedure.

## Milestone entry template

```text
## YYYY-MM-DD: Concrete milestone

Change: what now works or what was learned.
Evidence: completed report or comparison, summarized without private data.
Scope: generated, compiled, executed or validated, with the tested boundary.
Limitations: unresolved differences, failures and remaining work.
```

Keep full local evidence paths in private preparation notes outside the publication directory. Public pages should stand on their own without links to inaccessible private files.
