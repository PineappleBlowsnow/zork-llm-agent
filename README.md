# Playing Text Adventures with a Tool-Using LLM Agent

**MVA course — LLMs for Code and Proof**

An MVA coursework project, extending a course-provided ReAct agent for text-adventure games. The agent uses a Qwen language model to choose actions, then interacts with a Jericho game environment through MCP tools.

The implementation focuses on a practical failure mode: an agent repeatedly choosing an action that does not work. I extended the reference agent with location-aware failure memory, stagnation detection and fallback exploration. The published version is identified in [provenance](docs/PROVENANCE.md).

## My contribution

Starting from the course's working ReAct example, I added:

- **Failed-action memory:** records failures by location; non-movement failures are also stored globally.
- **Stagnation detection:** tracks steps without apparent progress and consults the map or memory tool after repeated stalls.
- **Fallback exploration:** replaces a repeated failed action with an untried direction or an available state-inspection tool.
- **Broader command handling:** recognizes diagonal movement and common failure messages, including Lost Pig responses.

The underlying ReAct loop, MCP server, game wrapper, reference agent and evaluation framework come from the course scaffold. In particular, the submitted MCP server is identical to the course example. See [provenance](docs/PROVENANCE.md) for the precise source boundaries.

## How it works

```text
Observation + recent history
            |
            v
Qwen -> THOUGHT / TOOL / ARGS
            |
            v
Normalize and validate -> failure-memory / stagnation checks
            |
            v
MCP tools: play_action, memory, get_map, inventory
            |
            v
Jericho environment -> observation, score, action history
```

The preserved coursework configuration calls `Qwen/Qwen2.5-72B-Instruct` through Hugging Face inference with temperature 0.0 and a step-dependent seed. Access to that model and provider support are external requirements; availability and deterministic behavior are not guaranteed by this repository.

## Layout

| Path | Purpose |
| --- | --- |
| `submission/agent.py` | Final course agent with failure-memory and recovery extensions |
| `submission/mcp_server.py` | Course MCP tools used by the agent |
| `example_submission/` | Unmodified course reference implementation |
| `games/` | Jericho wrapper; no game binaries |
| `evaluation/` | Course multi-trial evaluation and result aggregation |
| `run_agent.py` | Course command-line runner |
| `docs/` | Attribution and per-file source manifest |

## Setup and run

Use a Python 3.10+ environment with a working Jericho installation. Linux or WSL is a practical environment for the native game interpreter.

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Copy `.env.example` to `.env` and supply your own `HF_TOKEN`. The token is read from the environment, and `.env` is ignored by Git. Running the agent makes inference requests to your configured Hugging Face account.

Game binaries are not included. Obtain games you are entitled to use separately and place their `.z3`, `.z4`, `.z5` or `.z8` files under `z-machine-games-master/jericho-game-suite/`. The [course README](docs/COURSE_README.md) points to the BYU-PCCL game collection; review the individual games' terms before obtaining them.

```bash
python run_agent.py --list-games
python run_agent.py --agent submission --game zork1 --max-steps 100 --verbose
python -m evaluation.evaluate --submission ./submission --game zork1 --trials 3 --max-steps 100 --output results/zork1.json
```

Create `results/` before using an output path there. To compare against the supplied reference implementation, add `--reference` to the evaluation command. Explicitly select `--agent submission`: the original runner defaults to the course reference agent.

## Results and limitations

No verified score, win rate or improvement over the reference baseline is claimed. This publication was checked for Python syntax and package structure; no game or model evaluation was rerun.

The source preserves the coursework behavior, including limitations useful for further experiments:

- Global non-movement failure memory can incorrectly suppress an action that later becomes valid elsewhere.
- Failure detection and location extraction use text heuristics; an observation's first line is not always a room name.
- Fallback movement uses a fixed ordering rather than a learned exploration policy.
- `max_score=350` is hardcoded in the agent, and `game_completed` reflects a game-over heuristic rather than a verified win. Cross-game normalized scores require care.
- Reproducible comparisons still require a fixed environment, model/provider, game files, seeds and budgets; dependency versions were not locked in the original coursework.

## Attribution

The course scaffold is from the **MVA LLM for Code and Proof** text-adventure assignment. The preserved course history names Nathanaël Fijalkow as an upstream contributor. Its original README declares the MIT license and is included in [docs/COURSE_README.md](docs/COURSE_README.md). Game binaries, credentials, model weights and runtime logs are excluded.

The earlier [Hugging Face Space](https://huggingface.co/spaces/jiny05071225/zork_gold) is a related project link; this GitHub release is tied to the source commit documented below, rather than a claim that the Space currently runs an identical version.


[Back to Ying Jin's project directory](https://github.com/PineappleBlowsnow#mva-projects)
