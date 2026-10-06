---
mode: agent
description: Configure this checkout's Delphi LSP MCP server for GitHub Copilot
---

Set up this repository's Delphi LSP MCP server for GitHub Copilot on this
computer. This is a machine-specific setup; do not assume Delphi, MSBuild, or
DelphiLSP is installed at the same paths as another developer's installation.

1. Before cloning, ask the developer where the repository should go. Offer
   these choices using the available user-question mechanism:
   - GitHub Copilot's default repository directory (recommended)
   - A directory the developer provides
   If the developer chooses Copilot's default, use the actual configured
   default directory on this computer; do not guess or hard-code a path. If
   they choose a custom directory, ask them to provide its full path. Clone
   `https://github.com/SkybuckFlying/Delphi-LSP-MCP-Server.git` into the chosen
   directory if it is not already checked out there, then have the developer
   open that checkout in Copilot before continuing. Confirm the checkout's
   `origin` is the URL above, fetch updates, and identify the GitHub default
   branch. If the checkout is clean and only behind the default branch,
   fast-forward it. Do not discard, overwrite, or rebase local work. If the
   working tree is dirty, the branch has diverged, or the remote/default branch
   cannot be verified, stop and explain what needs the developer's attention.
2. Inspect `README.md`, `.mcp.json`, `DelphiLSPMCPServer.dproj`, and
   `DelphiLSPMCPServer.dpr` to confirm the current build output and supported
   command-line arguments. Preserve unrelated user configuration.
3. Locate the installed RAD Studio `rsvars.bat`, MSBuild, and `DelphiLSP.exe`.
   Prefer installed tool discovery and standard Embarcadero installation
   locations. If multiple Delphi LSP executables are available, use one
   compatible with this machine's installed Delphi version. Do not use a
   repository-local LSP binary or assume a path from another computer.
4. Determine the Windows machine architecture and build the project for the
   matching RAD Studio platform: Win64 on a 64-bit machine, Win32 on a 32-bit
   machine. Use the installed RAD Studio environment and MSBuild, with
   `Config=Release` and the selected platform. Do not build Debug or assume
   Win64. Keep build output ignored and do not commit generated executables or
   compiler artifacts.
5. Locate the executable produced by that Release build from the project
   configuration/build output; do not assume a Debug path or a fixed output
   directory. Configure the root `.mcp.json` in Copilot's portable MCP format
   with a `delphi-lsp` stdio server. Use the built Release executable, the
   discovered absolute path to `DelphiLSP.exe` as the `--lsp-path` value,
   `${workspaceFolder}` as the `--workspace` value, and `--log-level error` so
   diagnostics do not interfere with MCP stdio. Preserve other existing MCP
   servers if present.
6. Validate that the JSON parses and that both configured executable paths
   exist. Run the Release MCP server's `--help` to confirm it starts. Do not
   launch `DelphiLSP.exe` by itself because language servers normally wait for
   their LSP client on stdio.
7. Summarize the discovered installation paths, selected platform, Release
   build/configuration outcome, and any missing prerequisites. Do not claim
   the LSP integration works end-to-end unless Copilot has actually started
   the configured MCP server.

If Delphi or DelphiLSP cannot be found, stop before writing a broken
configuration and explain what the developer needs to install or provide.
