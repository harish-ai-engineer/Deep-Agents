# Deep Agents Development Workspace

An independently maintained development fork of
[LangChain Deep Agents](https://github.com/langchain-ai/deepagents). This
repository contains the Deep Agents Python SDK, terminal coding agent, CLI,
protocol support, integrations, evaluation tools, and examples in one
monorepo.

## Features

- Long-running agents with planning and context management
- Sub-agent delegation with isolated context
- Filesystem and shell tools
- Interactive terminal coding agent
- Multiple LLM provider integrations
- Persistent conversations and memory
- MCP and Agent Client Protocol support
- Local, sandboxed, and remote execution backends

## Repository Layout

```text
libs/
|-- deepagents/   Core Python SDK
|-- code/         Interactive terminal coding agent (`dcode`)
|-- cli/          Deployment CLI
|-- acp/          Agent Client Protocol support
|-- evals/        Evaluation suite
`-- partners/     Partner integrations

examples/         Example agents and deployment patterns
```

## Requirements

- [uv](https://docs.astral.sh/uv/)
- Git
- Python 3.11 or newer (managed automatically by `uv`)
- An API key for the model provider you plan to use

Do not commit API keys. For file-based local configuration, copy the root
`.env.example` file to `libs/code/.env`; `.env` is ignored by Git.

## Run the Terminal Agent

### Windows PowerShell

```powershell
git clone <your-repository-url>
cd <repository-directory>\libs\code

uv sync --group test

$env:OPENAI_API_KEY = Read-Host "OpenAI API key"
uv run dcode
```

### macOS or Linux

```bash
git clone <your-repository-url>
cd <repository-directory>/libs/code

uv sync --group test

read -s OPENAI_API_KEY
export OPENAI_API_KEY
uv run dcode
```

The local `libs/deepagents` package is installed in editable mode, so SDK
changes are immediately available to the terminal agent.

## Develop the SDK

```bash
cd libs/deepagents
uv sync --group test
uv run --group test pytest tests/unit_tests
```

Run quality checks from the package you changed:

```bash
uv run --all-groups ruff check .
uv run --all-groups ruff format . --check
uv run --all-groups ty check deepagents
```

The package Makefiles provide equivalent `test`, `lint`, `format`, and
benchmark targets on systems with GNU Make installed.

## Configuration

The terminal agent supports OpenAI, Anthropic, Google, and other model
providers. Set only the keys required by your selected provider:

```dotenv
OPENAI_API_KEY=
ANTHROPIC_API_KEY=
GOOGLE_API_KEY=
TAVILY_API_KEY=
```

Provider keys can be entered for the current shell session or stored in a
local `.env` file. Never place real credentials in `.env.example`, source
files, commits, issues, or chat messages.

## Testing

Unit tests do not require network access:

```bash
cd libs/code
uv run --group test pytest -n auto --benchmark-disable --disable-socket \
  --allow-unix-socket tests/unit_tests
```

Integration tests can use external services and may require provider
credentials:

```bash
cd libs/code
uv run --group test pytest tests/integration_tests
```

## Security

Deep Agents follows a tool-boundary security model: an agent can perform any
action allowed by its configured tools. Use restricted credentials, review
tool permissions, and prefer isolated sandboxes for untrusted workloads.

If a credential is accidentally exposed, revoke it at the provider
immediately and replace it. Removing it from a later commit is not enough.

## Upstream and License

This repository is based on
[langchain-ai/deepagents](https://github.com/langchain-ai/deepagents) and is
distributed under the [MIT License](LICENSE). Original copyright and license
notices are preserved.

To pull future upstream changes after cloning this fork:

```bash
git remote add upstream https://github.com/langchain-ai/deepagents.git
git fetch upstream
```
