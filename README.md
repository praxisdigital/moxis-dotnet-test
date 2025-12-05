# .NET Test Action

A reusable composite GitHub Action that checks out your repository, sets up .NET, restores dependencies, runs tests with the TRX logger, and publishes detailed test results with charts and annotations.

Ideal for any .NET (Core / .NET 5+) solution or project using `dotnet test`.

## Usage

### Basic Example (recommended)

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    name: Run .NET Tests
    steps:
      - name: Checkout
        uses: actions/checkout@v6

      - name: Run .NET Tests & Publish Results
        uses: your-username/moxis-dotnet-test@v1
        with:
          dotnet-version: 10.x   # optional, defaults to 10.x
```

### Override .NET Version

```yaml
      - uses: your-username/moxis-dotnet-test@v1
        with:
          dotnet-version: 8.0.x
```

### Full Workflow Example

```yaml
name: CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v6
        with:
          fetch-depth: 0   # Recommended for accurate test result annotations

      - name: Run tests and publish results
        uses: your-username/moxis-dotnet-test@v1
        with:
          dotnet-version: 10.x
```

## Inputs

| Input             | Description                              | Required | Default  |
|-------------------|------------------------------------------|----------|----------|
| `dotnet-version`  | The .NET SDK version to use (e.g. `8.0.x`, `10.x`) | No       | `10.x`   |

## Outputs

This action has no outputs, but it automatically publishes beautiful test result summaries using [EnricoMi/publish-unit-test-result-action](https://github.com/EnricoMi/publish-unit-test-result-action), including:

- Total tests, passed, failed, skipped
- Duration and trend charts
- Failure details and stack traces
- Direct links to failed tests in the Checks tab
- Pull request comment with summary (on PRs)

## Features

- Zero configuration needed in most cases
- Locked dependency restore (`--locked-mode`)
- No build step before test (faster runs when build is done elsewhere)
- Always publishes test results, even on failure or cancellation
- Works perfectly with pull requests and branch pushes
- Supports .NET 6, 7, 8, 9, 10+
