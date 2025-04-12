# buildkite-mcp-server

[![Build status](https://badge.buildkite.com/79fefd75bc7f1898fb35249f7ebd8541a99beef6776e7da1b4.svg?branch=main)](https://buildkite.com/buildkite/buildkite-mcp-server)

This is an [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) server for [Buildkite](https://buildkite.com). The goal is to provide access to information from buildkite about pipelines, builds and jobs to tools such as [Claude Desktop](https://claude.ai/download), [GitHub Copilot](https://github.com/features/copilot) and other tools, or editors.

# Tools

* `get_pipeline` - Get details of a specific pipeline in Buildkite
* `list_pipelines` - List all pipelines in a buildkite organization
* `list_builds` - List all builds in a pipeline in Buildkite
* `list_pipelines` - List all pipelines in a buildkite organization

Example of the `get_pipeline` tool in action.

![Get Pipeline Tool](docs/images/get_pipeline.png)

# prerequisites

* [goreleaser](http://goreleaser.com)
* [go 1.24](https://go.dev)

# Troubleshooting 

Logs for the Buildkite MCP server are stored in the platform-specific directory:

* macOS: ~/Library/Application Support/buildkite-mcp-server/logs/
* Windows: %APPDATA%\buildkite-mcp-server\logs\
* Linux: ~/.config/buildkite-mcp-server/logs/

The name of the log file is generated from the template `{{hostname}}_{{username}}_{{timestamp:2006-01-02}}_pid{{pid}}.log` so the files are unique for different mcp servers.

# building

Build the binary.

```
make build
```

Copy it to your path.

# configuration

Create a buildkite api token with read access to pipelines.

```json
{
    "mcpServers": {
        "buildkite": {
            "command": "buildkite-mcp-server",
            "args": [
                "stdio"
            ],
            "env": {
                "BUILDKITE_API_TOKEN": "bkua_xxxxxxxx"
            }
        }
    }
}
```


## Disclaimer

This project is in the early stages of development and is not yet ready for use.

## License

This project is released under MIT license.
