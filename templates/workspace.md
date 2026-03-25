# Debate Workspace Structure

Create this directory tree at the workspace path:

```
{workspace}/
├── debate.config.json                   # Input: question, perspectives, constraints
├── 00-brief.md                          # Problem statement + context
├── 01-initial/                          # Initial conclusions (pre-debate)
│   ├── {role-1}.md
│   ├── {role-2}.md
│   └── {role-N}.md
├── 02-rounds/                           # Pitch rounds
│   ├── round-1-{role-1}/
│   │   ├── pitch.md                     # Speaker's defense
│   │   ├── critique-{role-2}.md         # Each other agent's critique
│   │   ├── critique-{role-N}.md
│   │   └── rebuttal.md                  # Speaker's response to all critiques
│   └── round-N-{role-N}/
│       └── ...
├── 03-revised/                          # Post-debate revised conclusions
│   ├── {role-1}.md
│   └── {role-N}.md
├── corrections/                          # User fact-checks between rounds
│   ├── round-1.md                       # Confirmed/Corrected/New context
│   └── round-N.md
├── 04-synthesis.md                      # Facilitator's synthesis
├── 05-final/                            # Final conclusions
│   ├── {role-1}.md
│   ├── {role-N}.md
│   └── consensus.md                     # Facilitator's consensus document
└── debate.result.json                   # Structured output (machine-readable)
```

## Notes

- Agent count is dynamic — create one file per agent, not a fixed number
- The facilitator writes: `00-brief.md`, `debate.config.json`, `04-synthesis.md`, `consensus.md`, `debate.result.json`
- Each agent writes: their files in `01-initial/`, `02-rounds/`, `03-revised/`, `05-final/`
- All paths must be absolute when communicated to agents
