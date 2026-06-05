# blueprints.vault_integrations.oidc_auth

Configure a Vault OIDC auth backend against an existing IdP.

## Deployment model

`oidc_auth` (integration).

## Inputs

All configuration is supplied via caller variables — see `meta/argument_specs.yml`. Required inputs
have no default and fail fast when missing.

## Non-goals

- Does not retrieve secrets — the caller resolves them and passes them in
  (see [secret consumption standard](../../../../docs/standards/BLUEPRINTS_SECRET_CONSUMPTION.md)).
- Does not deploy or configure any other product. Cross-product wiring lives in
  `blueprints.vault_integrations` where applicable.
