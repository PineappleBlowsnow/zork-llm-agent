# Source and contribution record

Published from the local MVA text-adventure coursework archive on 2026-09-23.

- Final coursework commit: `ca1de1791d21a662b012986a9290a0a4c546b9cb` (`final work`, 2026-02-22, PineappleBlowsnow).
- Course baseline commit: `fb1ba557ae4e1244cbbe464899dc2c25f706b7ad` (immediately preceding the final coursework commit).
- `submission/agent.py` comes from `submission_template/agent.py` in the final commit. Compared with `example_submission/agent.py` in the course baseline, the source diff has 156 added and 5 removed lines; this is a provenance observation, not a measure of code quality or performance.
- `submission/mcp_server.py` matches the course example exactly at the source revision. It is credited as scaffold, not as an original MCP implementation.
- Runner, environment, evaluation and baseline files come from the committed course baseline. Uncommitted local model experiments in `example_submission/agent.py` are not presented as the final submission.
- The committed environment wrapper was selected because the local uncommitted version changed only game listing to another directory, leaving game loading inconsistent. No release logic was rewritten.
- All copied text files have line endings and trailing whitespace normalized. Per-file source and release hashes are in [source_manifest.json](source_manifest.json).
- The nested `zork_gold/` checkout is an older starter variant containing unimplemented methods. It is excluded.

## Attribution and license evidence

The original course README declares MIT and is preserved separately, including its historical instructions. The visible upstream history includes Nathanaël Fijalkow. No separate LICENSE file was present in the reviewed source tree; this release does not invent a copyright notice or claim that all scaffold code was authored by Ying Jin. Game and model licenses remain separate.

## Scope of checks

Python syntax, local path references, documented entry points and common credential patterns were checked without importing the model client, reading any `.env` value or running games. No training or inference was performed. No evaluation result files were found in the selected source snapshot, so no score is reported.
