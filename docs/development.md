# Development guide

## Requirements

- Python 3.11 or newer
- `uv`

## Setup

```bash
uv sync
uv run pre-commit install
```

To sync development dependencies run following command:

```bash
uv sync --group dev
```

## Try it out

Open a Python shell with the project's dependencies available:

```bash
uv run python
```

```python
import seriousdb

seriousdb.set("name", "Alice")
seriousdb.get("name")
```

## Nix

> The project provides a Nix development environment with the tools required for local development.

### Enter Nix develop

From the project root, run:

```bash
nix develop
```

This starts a shell with the project's development dependencies available.

### Nix (in the Nix Shell)

```bash
python
```

Then import and use `seriousdb` as shown above.

## Formatting

### Format Python files with `ruff`:

#### uv

```bash
uv run ruff format .
```

### Nix (in Nix Shell)

```bash
ruff format .
```

### To check formatting without changing files:

#### uv

```bash
uv run ruff format --check .
```

### Nix (in Nix Shell)

```bash
ruff format --check .
```

## Markdown formatting

Markdown files are formatted with [`mdformat`](https://github.com/hukkin/mdformat)
(GFM tables and task lists via
[`mdformat-gfm`](https://github.com/hukkin/mdformat-gfm)).
Both are pinned in the `dev` dependency group because the formatting style may
change between versions.

### Format Markdown files with `mdformat`:

Tracked Markdown files only (`git ls-files` skips ignored files such as
`.pytest_cache/README.md`, which a bare `mdformat .` would pick up after a
test run):

#### uv (bash)

```bash
uv run --locked mdformat $(git ls-files '*.md')
```

#### uv (PowerShell)

```powershell
uv run --locked mdformat (git ls-files '*.md')
```

### To check formatting without changing files:

#### uv (bash)

```bash
uv run --locked mdformat --check $(git ls-files '*.md')
```

#### uv (PowerShell)

```powershell
uv run --locked mdformat --check (git ls-files '*.md')
```

## Linting

### Lint python files with `ruff`:

#### uv

```bash
uv run ruff check .
```

#### Nix (in Nix Shell)

```bash
ruff check .
```

### Type checking with `ty`

#### uv

```bash
uv run ty check .
```

#### Nix (in Nix Shell)

```bash
ty check .
```

### To fix linter errors and warning if possible run following command:

#### uv

```bash
uv run ruff check --fix .
```

### Nix (in Nix Shell)

```bash
ruff check --fix .
```

## Docstrings

Python docstrings follow the [NumPy style](https://numpydoc.readthedocs.io/en/latest/format.html).

Docstrings are required for public functions, classes and modules only; for private ones (prefixed
with `_`) they are optional. This is enforced by `ruff` (see [Linting](#linting)).

A docstring starts with a one-line summary, followed by the sections that apply, such as
`Parameters`, `Returns` and `Raises`:

```python
def select(self, key: str) -> str:
    """Return the value stored under `key`.

    Parameters
    ----------
    key : str
        Key to look up.

    Returns
    -------
    str
        The value stored under `key`.

    Raises
    ------
    ResourceNotFoundError
        If `key` does not exist.
    ServiceUnavailableError
        If no database has been loaded.
    """
```

See [`cache.py`](../src/seriousdb/cache.py) and [`api.py`](../src/seriousdb/api.py) for more
examples.
