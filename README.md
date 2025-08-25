[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/Andresw2/blux-quantum/releases)

# BLUX Quantum — AI CLI for Creative Coding and Automation 🚀

![Space CLI](https://images.unsplash.com/photo-1446776811953-b23d57bd21aa?auto=format&fit=crop&w=1400&q=80)

Tags: ai · automation · blux · cli · coding · futuristic · modular · open-source · plugin · quantum · space-age · standalone · workflows

Overview
--------
BLUX Quantum is a standalone, space-age AI command line tool for creative coding and automation. It acts as a modular hub that runs locally, connects to the BLUX plugin ecosystem, and powers workflow automation for artists, developers, and ops teams.

The tool focuses on:
- Fast local CLI workflows.
- Modular plugins that extend I/O, transforms, and integrations.
- Simple scripting for generative and reactive tasks.
- Secure, offline-capable execution when possible.

This repository holds the project manifest, docs, and release links. Download the release asset from the Releases page and run the binary to start the CLI: https://github.com/Andresw2/blux-quantum/releases

Why BLUX Quantum
----------------
- Small, single-binary distribution for easy installs.
- Plugin-first design lets you load only what you need.
- Built for creative coding: chains, transforms, generative outputs.
- Automation primitives for job orchestration and event-driven runs.
- Works well in CI, local dev, and edge environments.

Badges
------
[![License](https://img.shields.io/badge/License-MIT-green)](LICENSE) [![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey)]()  
[![Topics](https://img.shields.io/badge/Topics-ai%2Cautomation%2Ccli-orange)]() [![BLUX](https://img.shields.io/badge/BLUX-Modular-purple)]()

Quick start — download and run
------------------------------
This repo provides prebuilt release files. You must download the release asset and execute it.

1. Open the Releases page and pick the latest build:
   https://github.com/Andresw2/blux-quantum/releases

2. Download the asset that matches your OS. The release contains a single executable or an archive with a binary named blux-quantum.

3. Run the executable:
   - Linux / macOS
     ```
     chmod +x blux-quantum
     ./blux-quantum --help
     ```
   - Windows (PowerShell)
     ```
     .\blux-quantum.exe --help
     ```

If you automate the fetch, use curl or wget to download the file from the release asset URL, then run it. The Releases page lists each asset. Download the correct file and execute it.

Examples and core commands
--------------------------
BLUX Quantum uses a concise CLI syntax. Commands accept subcommands, flags, and JSON or YAML for structured inputs.

Basic commands
- blux-quantum init
  - Create a local config and plugin folder.
- blux-quantum run <workflow>
  - Execute a named workflow.
- blux-quantum plugin list
  - Show installed plugins.
- blux-quantum plugin add <name>
  - Install a plugin from the BLUX registry.
- blux-quantum shell
  - Open an interactive session for live coding.

Example: run a generative workflow
```
blux-quantum run neon-loop --input input.json --out ./renders
```
Example: chain transforms
```
blux-quantum run chain \
  --steps '["decode:img", "stylize:neon", "export:png"]' \
  --out out/
```

Workflows and plugins
---------------------
Workflows are YAML or JSON files that define a sequence of nodes. Each node maps to a plugin and a task.

Example workflow (YAML)
```yaml
name: neon-loop
nodes:
  - id: load
    plugin: fs-loader
    params:
      path: assets/seed.png
  - id: stylize
    plugin: style-neon
    params:
      intensity: 0.6
  - id: save
    plugin: fs-writer
    params:
      path: renders/neon.png
```

Plugin model
- Plugins implement a small interface: init, run, teardown.
- Plugins can be native binaries or script adapters.
- Plugins register capabilities: input types, output types, resource hints.

Common plugins
- fs-loader / fs-writer — file I/O
- style-neon — generative style transform
- http-adapter — remote API integration
- watch — file system watcher that triggers workflows

Extending BLUX Quantum
----------------------
Add a plugin:
- Place the plugin binary or adapter in ./plugins.
- Update blux-config.yaml with the plugin entry.
- Reload plugins: blux-quantum plugin sync

Write a plugin adapter
- Use the plugin API bindings (Go, Python, Node).
- Implement a small RPC contract over stdin/stdout.
- Return JSON payloads with status and output references.

Config file
-----------
blux-config.yaml highlights:
- plugin paths
- default runtime limits
- workflow directories
- logging level

Example:
```yaml
plugins:
  path: ./plugins
runtime:
  max_workers: 4
workflows:
  path: ./workflows
logging:
  level: info
```

Scripting and automation
------------------------
Use BLUX Quantum in scripts and CI pipelines. The CLI returns standard exit codes. Use JSON output for programmatic parsing.

Pipe pattern
```
blux-quantum run job.json --json | jq '.result'
```

Use crontab or systemd timers to schedule runs. Use the watch plugin for event-driven automation.

Security and sandboxing
-----------------------
The binary runs with minimal privileges by default. Plugins run under a sandbox, and the config controls allowed paths and network access. The project focuses on safe local execution for creative workflows that use local assets or trusted APIs.

Integrations
------------
BLUX Quantum ships connectors and adapters:
- Git: fetch assets and commit outputs.
- Cloud storage: S3 and compatible endpoints.
- Messaging: Slack and Webhooks for notifications.
- Local devices: MIDI, OSC for interactive sound and visuals.

Quick CI example (GitHub Actions)
```yaml
jobs:
  build-run:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Download BLUX Quantum
        run: |
          curl -L -o blux-quantum https://github.com/Andresw2/blux-quantum/releases/download/latest/blux-quantum-linux
          chmod +x blux-quantum
      - name: Run workflow
        run: ./blux-quantum run nightly-render --out output/
```
Replace the asset URL with the correct release asset link after you view the Releases page.

CLI reference (common flags)
----------------------------
- --help: Show help.
- --config <path>: Use a different config file.
- --out <path>: Set output folder.
- --json: Output machine-readable JSON.
- --dry-run: Validate workflow without running.

Examples
--------
Generative art render
```
blux-quantum run generative-seed --params '{ "seed": 42, "steps": 128 }' --out ./art
```

Automated backup
```
blux-quantum run backup --schedule daily --out s3://backups/blux
```

Live interactive mode
- Use blux-quantum shell to experiment with plugins and steps.
- Use the REPL to test transforms and chain plugins.

Developer notes
---------------
Repository layout (example)
- /docs       — extended docs and tutorials
- /plugins    — plugin adapters and examples
- /workflows  — sample workflows and recipes
- /bin        — release artifacts in CI
- README.md   — project readme
- blux-config.yaml — default config

Build tips
- Follow the plugin API contract for binary plugins.
- Keep plugin code small and stateless when possible.
- Use logging rather than print for structured output.

Testing
-------
- Unit test plugins with the plugin harness.
- Use the integration runner to validate full workflows.
- Test cross-platform behavior in CI with matrix builds.

Troubleshooting
---------------
If a plugin fails, the CLI returns an error JSON with:
- step id
- error code
- message
- logs

To debug:
- Increase logging: --config blux-config.yaml (set level: debug)
- Run the plugin directly with the adapter harness.
- Re-run with --dry-run to validate workflow structure.

Releases and downloads
----------------------
Get the official release files here: https://github.com/Andresw2/blux-quantum/releases

Visit that page, choose the proper asset for your platform, download the file, and execute it. The release assets include platform-specific binaries and checksums. Always pick the binary that matches your OS and architecture. The release page lists version tags and detailed notes.

Roadmap
-------
Planned items:
- Plugin registry and remote discovery.
- Enhanced sandboxing with process-level constraints.
- Visual workflow editor for rapid prototyping.
- Expanded device adapters: MIDI, camera, OSC.
- Template library for creative prompts and shaders.

Contributing
------------
- Fork the repo.
- Add a plugin or sample workflow.
- Open a pull request with tests and docs.
- Report issues with reproducible cases.

Code of conduct
---------------
This project follows a code of conduct. Be respectful and clear in issues and pull requests.

License
-------
MIT License. See LICENSE for details.

Contact
-------
Open issues or pull requests on GitHub. Use the Releases page to get binaries: https://github.com/Andresw2/blux-quantum/releases

Screens and media
-----------------
![Neon art](https://images.unsplash.com/photo-1515879218367-8466d910aaa4?auto=format&fit=crop&w=1400&q=80)