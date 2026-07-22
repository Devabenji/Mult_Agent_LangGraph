# Multi-Agent LangGraph Demo

A small Python project demonstrating how to coordinate multiple agents with
[LangGraph](https://github.com/langchain-ai/langgraph) and a Groq-hosted language
model. The workflow accepts a user request, asks a planner agent to break it into
steps, and passes those steps to an executor node to produce the final result.

## Workflow

```text
User request -> Planner -> Executor -> Final output
```

- **Planner:** uses the configured Groq model to produce a short task list.
- **Executor:** processes the task list and returns the completed result.
- **State:** carries `user_input`, `task_list`, and `final_output` between nodes.

## Requirements

- Python 3.12 or newer
- [`uv`](https://docs.astral.sh/uv/)
- A Groq API key
- VS Code with the Python and Jupyter extensions when running the notebooks

## Setup

### 1. Configure the API key

This project expects `GROQ_API_KEY` to be available as an environment variable.
For GitHub Codespaces, create a Codespaces secret named `GROQ_API_KEY`, grant it
access to this repository, and rebuild or recreate the Codespace so the secret is
injected into the environment.

The required variable is documented in `.env.example`:

```env
GROQ_API_KEY=
```

Never commit a real API key. Local `.env` files are excluded by `.gitignore`.

### 2. Install dependencies

From the project directory, run:

```bash
uv sync
```

This creates or updates `.venv` using the dependencies in `pyproject.toml` and
the locked versions in `uv.lock`.

### 3. Select the notebook kernel

Open a notebook in VS Code and select:

```text
.venv (Python 3.12.1)
```

If it is not listed, choose **Select Another Kernel** -> **Python Environments**
and select `.venv/bin/python`.

## Run the demo

Open [Multi_Agent_Langgraph_Demo.ipynb](Multi_Agent_Langgraph_Demo.ipynb) and run
the cells from top to bottom. The final cells invoke the graph with this example:

```python
state = {"user_input": "How do solar panels work?"}
final_state = graph_compiled.invoke(state)
print(final_state["final_output"])
```

Change `user_input` to try another request.

The repository also includes
[Multi_Agent_LangGraph.ipynb](Multi_Agent_LangGraph.ipynb), a more thoroughly
explained version of the same workflow with an additional finalizer node.

## Project structure

```text
.
├── Multi_Agent_Langgraph_Demo.ipynb  # Concise two-node demo
├── Multi_Agent_LangGraph.ipynb       # Guided three-node workflow
├── pyproject.toml                     # Project metadata and dependencies
├── uv.lock                            # Reproducible dependency lockfile
├── .env.example                       # Required environment variables
└── main.py                            # Minimal Python entry point
```

## Troubleshooting

- **`GROQ_API_KEY is not available`:** confirm the Codespaces secret name and
  rebuild the Codespace after adding or changing it.
- **`ModuleNotFoundError`:** run `uv sync`, then confirm that the notebook uses
  `.venv/bin/python` rather than a global Python kernel.
- **Kernel name does not refresh:** save and close the notebook, reopen it, and
  select the `.venv` interpreter again.
