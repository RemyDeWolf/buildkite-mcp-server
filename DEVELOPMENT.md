# Development

This contains some notes on developing this software locally.

# prerequisites

* [goreleaser](http://goreleaser.com)
* [go 1.24](https://go.dev)

# building

## Local Build

Build the binary locally.

```bash
make build
```

Copy it to your path.

## Docker

### Local Development

Build the Docker image using the local development Dockerfile:

```bash
docker build -t buildkite/buildkite-mcp-server:dev -f Dockerfile.local .
```

Run the container:

```bash
docker run -i --rm -e BUILDKITE_API_TOKEN="your-token" buildkite/buildkite-mcp-server:dev
```

# Adding a new Tool

1. Implement a tool following the patterns in the [internal/buildkite](internal/buildkite) package - mostly delegating to [go-buildkite](https://github.com/buildkite/go-buildkite) and returning JSON. We can play with nicer formatting later and see if it helps. 
2. Register the tool here in the [internal/stdio](internal/commands/stdio.go) file.
3. Update the README tool list.
4. Profit!

# Validating tools locally

When developing and testing the tools, and verifying their configuration https://github.com/modelcontextprotocol/inspector is very helpful.

```
make
npx @modelcontextprotocol/inspector buildkite-mcp-server stdio
```

Then log into the web UI and hit connect.

# Releasing to GitHub

To push docker images GHCR you will need to login, you will need to generate a legacy GitHub PSK to do a release locally. This will be entered in the command below.

```
docker login ghcr.io --username $(gh api user --jq '.login') 
```

Publish a release in GitHub, use the "generate changelog" button to build the changelog, this will create a tag for the release.

Fetch tags and pull down the `main` branch, then run GoReleaser at the root of the repository.

```
git fetch && git pull
GITHUB_TOKEN=$(gh auth token) goreleaser release
```