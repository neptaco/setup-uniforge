# setup-uniforge

GitHub Action to install [UniForge](https://github.com/neptaco/uniforge) CLI for Unity development.

## Usage

```yaml
steps:
  - uses: actions/checkout@v4

  - uses: neptaco/setup-uniforge@v1

  - run: uniforge editor install --from-project .
  - run: uniforge run -p . --ci -- -executeMethod Build.Execute
```

### With version

```yaml
# Full version
- uses: neptaco/setup-uniforge@v1
  with:
    version: v1.0.0

# Major version (latest v1.x.x)
- uses: neptaco/setup-uniforge@v1
  with:
    version: v1

# Minor version (latest v1.2.x)
- uses: neptaco/setup-uniforge@v1
  with:
    version: v1.2

# With token to avoid API rate limits
- uses: neptaco/setup-uniforge@v1
  with:
    version: v1
    token: ${{ secrets.GITHUB_TOKEN }}
```

### Build from source (development)

Build from a specific branch, tag, or commit SHA. Requires Go to be installed.

```yaml
- uses: actions/setup-go@v5
  with:
    go-version: '1.24'

- uses: neptaco/setup-uniforge@main
  with:
    ref: main  # branch, tag, or commit SHA
```

## Inputs

| Name | Description | Required | Default |
|------|-------------|----------|---------|
| `version` | UniForge version to install (`latest`, `vX.Y.Z`, `vX`, or `vX.Y`) | No | `latest` |
| `ref` | Build from source at this ref (branch, tag, or commit SHA). Requires Go. | No | `''` |
| `token` | GitHub token for API requests (use to avoid rate limits when using partial version) | No | `''` |

## Supported Platforms

| OS | Architecture |
|----|--------------|
| Linux | amd64, arm64 |
| macOS | amd64, arm64 |
| Windows | amd64 |

## License

MIT
