# Terrakube CLI Reference

`terrakube` brings Terrakube infrastructure automation, workspace lifecycle management, organization management, private registry, access control, and dynamic provider credentials directly to your terminal.

---

## Guide Index

- **[Installation](install.md)**: Download official release binaries for Linux, macOS, and Windows or build from source.
- **[Getting Started](getting-started.md)**: Login/Logout setup, authentication options, configuration files, global flags, and complete shell quickstart script.
- **[Command Reference](commands/README.md)**: Complete detailed CRUD reference for all CLI resources and subcommands.

---

## CLI Features

- **JSON:API Native Integration**: Full CRUD support for Organizations, Workspaces, Teams, Projects, Private Registry (Modules & Providers), Templates, Collections, Agents, SSH Keys, VCS, and OIDC Federated Identity.
- **Flexible Output Formats**: Render output as formatted tables (`--output table`), clean JSON (`--output json`), YAML (`--output yaml`), or TSV (`--output tsv`).
- **Flexible Parent Scopes**: Identify parent entities via UUID flags (e.g. `--organization-id`) or human-readable names (e.g. `--organization` / `-o`).
- **Automation Ready**: Fully scriptable via environment variables (`TERRAKUBE_API_URL`, `TERRAKUBE_TOKEN`, `TERRAKUBE_ORGANIZATION`).
