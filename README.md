# clanker-guard-agent releases

Published builds of the Clanker Guard endpoint agent, plus the installers that
verify them. This repository holds release artifacts only; the source lives in
the main Clanker Guard tree.

## Install (verified SHA-256, no service, no telemetry)

Linux / macOS:

```bash
curl -fsSL https://github.com/derekzacharias/clanker-guard-agent/releases/download/v0.1.1/install-agent.sh \
  | bash -s -- --version 0.1.1 --server http://<dashboard>:8787 --enroll-token <token>
```

Windows (PowerShell):

```powershell
iwr https://github.com/derekzacharias/clanker-guard-agent/releases/download/v0.1.1/install-agent.ps1 `
  -OutFile "$env:TEMP\install-agent.ps1" -UseBasicParsing
& "$env:TEMP\install-agent.ps1" -Version 0.1.1 -Server "http://<dashboard>:8787" -EnrollToken "<token>"
```

Each installer downloads the platform build from the tagged release, checks it
against the published SHA-256 (or `SHA256SUMS`), and only then installs it.
Nothing is installed as a service or scheduled task.

Pass `--server` and `--enroll-token` (the dashboard shows the full command under
Settings → Agents) and the endpoint enrolls, runs one assessment, and reports the
findings plus its progress to that server only. The server cannot send it
commands, and nothing is sent to a third party. Without those flags the agent is
purely local.

Uninstall:

```bash
clanker-guard-agent uninstall --yes
```

## Artifacts per release

| File | Platform |
| `clanker-guard-agent_linux_amd64` / `_linux_arm64` | Linux |
| `clanker-guard-agent_darwin_amd64` / `_darwin_arm64` | macOS |
| `clanker-guard-agent_windows_amd64.exe` / `_windows_arm64.exe` | Windows |
| `install-agent.sh`, `install-agent.ps1`, `uninstall-agent.sh` | installers |
| `SHA256SUMS`, `*.sha256` | checksums |

Build and publish from the main tree:

```bash
./scripts/build-agent-release.sh 0.1.1
gh release create v0.1.1 dist/v0.1.1/* --repo derekzacharias/clanker-guard-agent
```
