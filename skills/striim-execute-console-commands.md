---
name: striim-execute-console-commands
description: Execute Striim console or TQL DDL commands over REST (available since Striim 3.10.3).
api: Striim Application Management REST API (base path /api/v2 on your Striim server or Striim Cloud service)
operations:
  - TungstenCommandExecution
generated: 2026-09-03
method: generated
source: openapi/striim-tql-files-rest-api-5-4-0-2-openapi.yml
---

# Execute console and TQL commands

`POST /tungsten` (`TungstenCommandExecution`) executes a console or TQL DDL command — the REST
equivalent of the Striim console. Requires `Authorization: STRIIM-TOKEN <36-character token>`.

- Send the command text in the request body; responses are JSON. (Striim 4.2.0 fixed commands that
  previously returned invalid JSON — a breaking change if you consumed the malformed output.)
- Use it for DDL the resource endpoints do not cover: CREATE/ALTER/DROP of components, namespaces,
  and applications defined in TQL.
- There is no generic undo for arbitrary DDL: individual CREATE and DROP commands are each other's
  reversals. Prefer the typed endpoints (deployment, sprint, applications) where they exist, since
  those have explicit documented reversal operations.
