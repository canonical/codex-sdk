# Codex SDK for Workshop

This SDK provides the OpenAI Codex CLI for AI-assisted coding within a
workshop. The agent is sandboxed in the workshop container. Credentials are
persisted between workshop updates.

---

## Reference workshop

A minimal workshop:

```yaml
# workshop.yaml
name: codex
base: ubuntu@24.04
sdks:
  - name: codex
    channel: latest/stable

actions:
  codex-yolo: codex --yolo "$@"

  codex-yolo-exec: codex exec --yolo "$@"
```

This creates a basic Codex environment.
The agent is sandboxed by the workshop,
so interactive and non-interactive actions can use the YOLO mode.

---

## Using the SDK

### Prerequisites, project layout

1. No prerequisite SDKs are required.
2. Place your project files in your project directory. No special layout is
   required; Codex works with any codebase.
3. On launch, the SDK configures `PATH` for the `codex` binary
   and adds a `AGENTS.md` hint about the workshop environment.

### Start a coding session

Once the workshop is ready:

```bash
workshop shell
codex
```

This opens an interactive Codex session inside the workshop. You can ask
Codex to read files, write code, run commands, and navigate your project.

### Authenticate with Codex

To make your host OpenAI credentials available inside the workshop,
you have two alternatives:

- Set the `OPENAI_API_KEY` [environment variable](https://developers.openai.com/api/docs/quickstart/) inside the workshop.
  You can pass it using the `--env` option with `workshop run` or `workshop exec`,
  or by other means such as [direnv](https://direnv.net/).

- If the variable isn't set, Codex will prompt for an OpenAI API key
  or offer browser-based login on first interactive use or at `codex login`.
  The mount plug persists these credentials between workshop updates.

---

## Plugs (resources this SDK consumes)

### `codex-config`

- Interface: `mount`
- Workshop target: `/home/workshop/.codex`
- Purpose: Preserves Codex's credentials and settings between workshop updates.
  You can also use `workshop remount` to control its contents on the host.
  To mount your existing `~/.codex` settings into the workshop, stop
  the workshop first, remount, then start it again:

  ```bash
  workshop stop <workshop-name>
  workshop remount <workshop-name>/codex:codex-config ~/.codex
  workshop start <workshop-name>
  ```

## Slots (resources this SDK provides)

This SDK doesn't define any slots.

---

## Documentation and guidance

- [Codex official documentation](https://developers.openai.com/codex)
- [Workshop documentation](https://canonical-workshop.readthedocs-hosted.com/latest/)

---

## Community and support

- OpenAI community: [OpenAI Community Forum](https://community.openai.com/)
- Workshop forum:
  [Workshop Discourse](https://discourse.canonical.com/c/engineering/workshops/34)
- Please review our
  [Code of Conduct](https://ubuntu.com/community/ethos/code-of-conduct) before
  participating.

---

## Contributions

All contributions, including code, documentation updates, and issue reports,
are welcome!

- See `CONTRIBUTING.md` for guidelines.
- Open issues or pull requests on the official repository.

---

## License and copyright

Copyright 2026 Canonical Ltd.

This program is free software: you can redistribute it and/or modify it under
the terms of the
[GNU Lesser General Public License version 2.1 (LGPLv2.1)](https://www.gnu.org/licenses/old-licenses/lgpl-2.1.html)
as published by the Free Software Foundation.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

[Codex](https://github.com/openai/codex) is licensed under
the [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
