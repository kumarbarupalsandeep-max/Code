# Code — Deep Product & Market Research

**Owner / Content Creator:** Sandeep Barupal  
**Copyright:** © 2026 Sandeep Barupal. All rights reserved, except third-party resources under their own terms.  
**Research status:** Deep-research baseline v1.0 — desk research; editor scope must be validated with developers.

## 1. Product thesis
Code is envisioned as a web-based development workspace/editor. Its central product decision is whether to be a browser code editor, a cloud development environment, or a full IDE. Editing text is only one part of an IDE: users also expect project navigation, language intelligence, terminal/build tasks, debugging, source control, extensions and secure workspace management.

## 2. Competitor landscape
VS Code provides an extensible editor ecosystem; Monaco is the browser-based editor component derived from VS Code; GitHub Codespaces supplies cloud-hosted compute and project environments usable through browser or VS Code. These are different layers, not direct substitutes. Sources: [VS Code docs](https://code.visualstudio.com/docs), [Monaco Editor](https://microsoft.github.io/monaco-editor/), [GitHub Codespaces](https://docs.github.com/en/codespaces).

| Layer | Reference | What to study |
|---|---|---|
| Editor UI component | Monaco | Editing, language services, accessibility, browser performance |
| Desktop editor ecosystem | VS Code | Extensions, settings, debugging, source control, workspace model |
| Remote development | Codespaces | Environment provisioning, devcontainers, persistence, quotas |
| Browser sandbox IDEs | Replit and similar | Onboarding, preview, sharing, execution isolation and collaboration |

## 3. Scope recommendation
Start with a reliable browser editor and project workspace; defer arbitrary code execution until isolated infrastructure exists. Never run user code inside the web application process or with platform credentials.

## 4. Feature specification
### P0
- Workspace/project list, file tree, tabs, editor, search and command palette.
- Syntax highlighting, configurable theme, keyboard shortcuts, autosave and unsaved-change recovery.
- Import/export project files; Git repository connection with read-only mode first.
- Accessible keyboard navigation and responsive layout.
- Clear runtime support labels: editing-only vs executable workspace.
### P1
- Language server integration, diagnostics, formatting, snippets and symbol navigation.
- Terminal/build/test tasks inside isolated workspaces.
- Git status, diff and commit workflows with confirmation.
- Preview server with port proxy and safe URL handling.
### P2
- Debugging, collaborative editing, extension marketplace, AI assistance with code provenance and enterprise policies.

## 5. Page map
Workspace Home; Project Picker; Editor Workspace; Search; Source Control; Branch/Diff; Terminal; Run/Preview; Debug; Extensions; Settings; Account/Sessions; Usage/Quota; Audit/Admin.

## 6. Architecture
Separate browser editor, workspace API, file service, language-service adapters and optional execution plane. For remote execution, create short-lived isolated containers/VMs, enforce CPU/memory/time/disk/network limits, block cloud metadata endpoints, use non-root identities, mount minimal secrets, and destroy workspaces safely. Store project data with versioning and recovery. Use WebSocket reconnect/backpressure controls for terminals and language services.

## 7. Security
Treat code and project files as untrusted. Apply strict CSP, safe file path handling, upload limits, malware scanning where appropriate, secret redaction, tenant isolation and audit logs. Extension installation requires trust boundaries and publisher review. Prevent SSRF through preview proxies and terminal/network controls. Do not claim sandboxing until it has been independently tested.

## 8. UX and performance metrics
Time to open a project, editor responsiveness on large files, autosave recovery success, language-server latency, workspace start time, preview success, crash-free sessions and keyboard task completion. Support reduced motion, high contrast, screen readers and low-bandwidth recovery.

## 9. Risks and validation
Risks: scope explosion, slow editor, browser memory limits, unsafe code execution, extension compromise and cloud compute costs. Test with real project sizes and different network conditions. Interview frontend/backend developers about must-have workflows before building a terminal or debugger.

## 10. References
- [Visual Studio Code documentation](https://code.visualstudio.com/docs)
- [Monaco Editor](https://microsoft.github.io/monaco-editor/)
- [GitHub Codespaces](https://docs.github.com/en/codespaces)
- [OWASP Cheat Sheet Series](https://cheatsheetseries.owasp.org/)

## 11. Next work
Decide the product boundary, compare browser editor vs cloud IDE requirements, prototype the core workspace, and threat-model any code execution before implementation.

## 15. Deep research addendum — evidence and product decisions

### Market evidence
VS Code documents an extensible editor with integrated terminal, source control, debugging, settings, and extensions. Its source-control guide notes that Git operations such as staging, commits, branches, and merge conflict resolution are available in the editor while using the same underlying repository as command-line Git. Monaco Editor is a separate browser-based editor component and should not be mistaken for a complete IDE/runtime. Sources: [VS Code overview](https://code.visualstudio.com/docs/editing/getting-started/overview), [Source control](https://code.visualstudio.com/docs/sourcecontrol/overview), [Monaco Editor](https://microsoft.github.io/monaco-editor/).

### Competitive implications
A web-based Code workspace needs to choose its execution model early: local browser editing, remote development container, or managed cloud workspace. Editing text in a browser is far simpler than safely executing arbitrary code. The MVP should clearly state runtime and language limits rather than imply a full desktop IDE.

### Recommended core workflows
1. Create/import a project from a template or Git repository.
2. Browse files, edit with syntax highlighting, search, and safe autosave/recovery.
3. Inspect diffs, stage changes, commit, branch, pull, and push through a documented Git integration.
4. Run a controlled preview/build in an isolated workspace with logs and resource quotas.
5. Provide diagnostics and task output; add debugging only for supported runtimes.
6. Manage extensions/plugins through a curated allowlist and signed/versioned metadata.

### Architecture and security
Keep the editor UI separate from workspace execution. Run user code in ephemeral isolated containers/VMs with CPU, memory, time, filesystem, and network limits. Use short-lived credentials and never expose repository tokens to browser JavaScript. Add audit logs, workspace expiry, recovery, and explicit permissions for terminal/network access.

### Validation and success measures
Measure time to first successful preview, workspace startup latency, save/recovery reliability, build success, active project retention, and support incidents. Test with realistic repositories, large files, poor networks, and concurrent sessions.

### Sources and caveats
VS Code is a reference for editor workflows, not a license to copy its brand, icons, or UI. Monaco is an editor component, not a turnkey cloud IDE. Language support, terminal access, and collaboration require separate architecture and cost analysis.
