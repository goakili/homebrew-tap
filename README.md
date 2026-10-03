# Akili Homebrew Tap

Homebrew formulae for [Akili](https://github.com/goakili/akili), the security-first control plane
for autonomous AI operator agents.

Works on **macOS and Linux**, on both Apple Silicon/arm64 and x86_64.

## Install

```sh
brew install goakili/tap/akili-agent
```

…or tap once and install by name. Homebrew 6 requires third-party taps to be
explicitly trusted, so `brew trust` is needed for the short form:

```sh
brew tap goakili/tap
brew trust goakili/tap
brew install akili-agent
```

## Run the agent

Add an agent in your control plane (**Agents → Add agent**) and copy its one-time join token. Enroll
once, then run the agent as a background service that starts at login:

```sh
AKILI_JOIN_TOKEN=akj_... akili-agent enroll --url https://akili.example.com --state-dir "$(brew --prefix)/var/akili-agent"
brew services start akili-agent
```

The service keeps its state (the agent's key and its working directory) in
`$(brew --prefix)/var/akili-agent` and logs to `$(brew --prefix)/var/log/akili-agent.log`.

A control plane with a self-signed or private-CA certificate needs its CA:
add `--ca-cert ca.pem` to `enroll`. The agent keeps a copy in its state directory.

On macOS, host tools that need systemd are unavailable; coding tasks work, and `sandbox_exec` needs
Docker Desktop. For servers, prefer the Linux install from the control plane, which runs the agent
as a hardened systemd service.

## Packages

| Formula | Description | Source |
|---|---|---|
| `akili-agent` | The Akili agent | [goakili/akili](https://github.com/goakili/akili) (`agent/`) |

## Upgrade

```sh
brew update && brew upgrade akili-agent
brew services restart akili-agent
```

The formula follows the latest release. Keep the agent on the same version as your control plane.

## Uninstall

```sh
brew services stop akili-agent
brew uninstall akili-agent
brew untap goakili/tap   # optional: remove the tap too
```

The state directory stays in `$(brew --prefix)/var/akili-agent`; delete it to remove the agent's key.

## Other install methods

- **Linux server:** the install command shown by the control plane (`install-agent.sh`).
- **Docker:** `docker run … jkaninda/akili-agent:<version>`, also shown by the control plane.
- **Binaries** for every platform are on the
  [releases page](https://github.com/goakili/akili/releases/latest).
