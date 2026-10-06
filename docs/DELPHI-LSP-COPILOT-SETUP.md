# Set up Delphi LSP for GitHub Copilot

This guide configures the repository's Delphi LSP MCP server for GitHub
Copilot on a Windows developer machine. The MCP server and Delphi LSP executable
run locally; their paths and build outputs are specific to each computer.

## Prerequisites

- Git and GitHub Copilot in a supported client, such as Visual Studio Code.
- Embarcadero RAD Studio / Delphi installed, including the Delphi command-line
  compiler and `DelphiLSP.exe`.
- Permission to run the repository's local MCP server through Copilot.

## Choose where to clone the project

Start the setup prompt in GitHub Copilot. It asks whether to use Copilot's
default repository directory or a directory you provide:

- **Copilot's default repository directory:** Copilot uses the configured
  default location on this computer.
- **A directory I provide:** Enter the full path to the directory where you
  want the project cloned.

Copilot clones the latest project from:

```text
https://github.com/SkybuckFlying/Delphi-LSP-MCP-Server.git
```

After cloning, open the new checkout as the active Copilot project and run the
setup prompt there. If you already have a checkout in the chosen directory,
Copilot should verify that it has the correct `origin`, fetch the GitHub default
branch, and fast-forward only when there are no local changes or diverging
commits. Preserve local work and resolve a dirty or diverged checkout before
setup.

## Run the Copilot setup prompt

Open the checkout in Visual Studio Code with GitHub Copilot Chat, then run the
repository prompt:

```text
/setup-delphi-lsp
```

The prompt is stored at
`.github/prompts/setup-delphi-lsp.prompt.md`. It checks that the checkout is
current, discovers the locally installed Delphi build tools and `DelphiLSP.exe`,
selects Win32 or Win64 based on the machine, builds the MCP server using the
installed RAD Studio in **Release** configuration, and configures the root
`.mcp.json` to use that build's actual output path.

Review any shell commands before allowing Copilot to run them. If the prompt is
not listed, verify that the repository is open as the workspace and that the
client supports repository custom prompts.

## Verify the setup

The setup should leave `.mcp.json` configured with a `delphi-lsp` stdio server,
the local `DelphiLSPMCPServer.exe`, and the machine-specific absolute path to
`DelphiLSP.exe`. Build output is local and must not be committed. Copilot should
start the MCP server from its MCP controls; the standalone Delphi language
server is not meant to be launched directly because it communicates with its
client over stdio.
