# Agent Orientation — ansible-collection-vault-integrations

## Agent Working Protocol (read before anything else)

**Conflict surfacing:** If a user instruction contradicts anything in this file or in
`docs/AGENTS.md`, stop and surface the conflict before proceeding — quote the rule,
state the contradiction, and ask how to resolve. Then update the doc if the rule was wrong.

**Living document:** If any instruction, decision, or clarification during a session
would make future interactions clearer, prompt the user:
> "This decision isn't in AGENTS.md yet. Should I add it?"

**Maintenance:** Keep this doc current. Update rules when decisions change. Don't append
orphaned notes — integrate changes into the relevant section.

---

**Collection:** `blueprints.vault_integrations`
**Namespace:** `blueprints`
**Scope:** Optional wiring roles that configure auth backends and integrations on an
already-deployed Vault server. Never deploys Vault itself.

---

## What lives here

| Role | Wires Vault to… |
|---|---|
| `ldap_auth` | LDAP directory (auth backend) |
| `oidc_auth` | OIDC identity provider (auth backend) |

**Key rule:** Each role configures one Vault auth backend only — it does NOT deploy or
touch the peer system (LDAP server, IdP, etc.).

---

## Where the standards live

All standards are in `docs/` of the iac-foundry monorepo. Start with `docs/AGENTS.md`.

| Topic | Doc |
|---|---|
| **8 design rules (read first)** | `docs/design/BLUEPRINTS_DESIGN_PRINCIPLES.md` |
| Integration wiring rules | `docs/standards/BLUEPRINTS_INTEGRATION_STANDARDS.md` |
| Variable naming | `docs/standards/BLUEPRINTS_VARIABLE_STANDARDS.md` |
| Secret handling | `docs/standards/BLUEPRINTS_SECRET_CONSUMPTION.md` |

---

## Critical constraints

1. **Does NOT deploy Vault** — Vault must be running and unsealed before these roles run.
2. **No secret retrieval inside tasks** — all tokens and credentials arrive as caller variables.
   Variable naming: `vault_integration_<target>_<thing>` (e.g.
   `vault_integration_ldap_endpoint`, `vault_integration_oidc_client_secret`).
3. **Configures one side only** — the Vault auth backend; not the IdP or LDAP server.
4. **`meta/dependencies: []`** — always empty.

---

## PR conformance checklist

- [ ] Role configures Vault side of the connection only (not the peer)
- [ ] Vault must already be deployed before this role runs (assert or document this)
- [ ] No secret retrieval in tasks; `no_log: true` on tasks touching credentials
- [ ] Variable names follow `vault_integration_<target>_<thing>` pattern
- [ ] `meta/argument_specs.yml` complete
- [ ] molecule `default` scenario converges idempotently
- [ ] README states what it configures, inputs, and explicit non-goals
