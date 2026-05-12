# GEMRIC skills for LLMs

This is a brain-dump of one member of the GEMRIC project. May it help researchers that are starting to work on Safe servers.


## Setup

Duh - ask you favorite LLM to setup the skill files included in sub-directories of this repository.


### Local setup

I prefer a local LLM setup using "LM Studio" (google/gemma-4-26b-a4b or qwen3.5) with a claude code plugin for visual studio code. Use the following environment variables (~/.zshrc):

```bash
export ANTHROPIC_AUTH_TOKEN=lmstudio
export ANTHROPIC_BASE_URL=http://localhost:1234
```

If you see error messages about 'xhigh' not existing select the high option (/effort) or edit your .claude/settings.json file to look like this:

```json
{
  "effortLevel": "max"
}
```
