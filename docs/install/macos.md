---
summary: "Install OpenClaw on macOS — Homebrew, Node 22, installer script, and companion app"
read_when:
  - Installing OpenClaw on a Mac for the first time
  - You hit PATH or sharp/libvips errors after a macOS npm install
title: "macOS"
---

# Install on macOS

The fastest path on a Mac is the **installer script**, which handles Homebrew,
Node, and onboarding in one step.

## Recommended: installer script

Open **Terminal** and run:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

The script will:

1. Install Homebrew if it is missing.
2. Install Node 22 via Homebrew if needed.
3. Install `openclaw` globally via npm.
4. Launch the onboarding wizard.

To skip onboarding and just get the binary:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

Full flag reference: [Installer internals](/install/installer).

## Alternative: npm / pnpm / bun

If you already have Node 22 and prefer to install manually:

```bash
# npm
npm install -g openclaw@latest

# pnpm
pnpm add -g openclaw@latest

# bun
bun add -g openclaw@latest
```

Then run the onboarding wizard:

```bash
openclaw onboard --install-daemon
```

## Step 0 — install Node 22 (if needed)

Check your current version:

```bash
node -v
```

If it is below `v22` or missing, install it:

```bash
brew install node
```

No Homebrew yet?

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Full Node install options: [Node.js](/install/node).

## Troubleshooting

### `openclaw: command not found`

macOS uses **zsh** by default. After a global npm install, you may need to
reload your shell:

```bash
source ~/.zshrc
```

If `openclaw` is still not found, add npm's global bin directory to your
PATH. Append this line to `~/.zshrc`:

```bash
export PATH="$(npm prefix -g)/bin:$PATH"
```

Then open a new terminal window.

### `sharp` build errors (libvips conflict)

If you have `libvips` installed via Homebrew and `sharp` fails to install,
force prebuilt binaries:

```bash
SHARP_IGNORE_GLOBAL_LIBVIPS=1 npm install -g openclaw@latest
```

If you see `sharp: Please add node-gyp to your dependencies`, install build
tooling first:

```bash
xcode-select --install
npm install -g node-gyp
```

### `EACCES` permission errors

These are rare on macOS but can happen if your npm global prefix is
root-owned. Fix:

```bash
npm config set prefix "$HOME/.npm-global"
export PATH="$HOME/.npm-global/bin:$PATH"
```

Add the `export` line to `~/.zshrc` to make it permanent.

## After install

Verify everything is working:

```bash
openclaw doctor          # health checks
openclaw gateway status  # gateway status
openclaw dashboard       # open the browser UI
```

## macOS companion app

For a native menu-bar experience, voice integration, and macOS-only tools
(Canvas, Camera, Screen Recording), download and install the **OpenClaw
macOS app**. The app can manage the Gateway service, request TCC permissions,
and install the CLI for you.

See [macOS companion app](/platforms/macos) for details.

## Related

- [Getting Started](/start/getting-started)
- [Installer internals](/install/installer)
- [Node.js install](/install/node)
- [macOS companion app](/platforms/macos)
- [Updating](/install/updating)
