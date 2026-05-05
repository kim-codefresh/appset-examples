# AppSet Template Examples

Examples for SIE-116 spike — understanding real-world AppSet template complexity to inform the path resolution approach.

## Examples

### `simple/` — basic variable substitution
Uses only `{{.path}}` and `{{.path.basename}}`.
**Option (a) handles this fine.**

### `multi-variable/` — multiple custom variables
Uses multiple variables from params.json (`{{.cluster}}`, `{{.app}}`, `{{.env}}`, etc.).
**Option (a) handles this fine.**

### `conditional/` — if/else directives
Uses `{{if eq .env "production"}}...{{else}}...{{end}}`.
**Option (a) cannot handle this — needs option (b) ArgoCD dry-run.**

### `range/` — range loops
Uses `{{range $k, $v := .labels}}...{{end}}`.
**Option (a) cannot handle this — needs option (b) ArgoCD dry-run.**

### `pipe-functions/` — pipe functions
Uses `{{.path | trimPrefix "/"}}`, `{{.app | replace "-" "_" | lower}}`.
**Option (a) cannot handle this — needs option (b) ArgoCD dry-run.**

## Key question for the spike

What % of real-world AppSets (that use Git File Generator) only use simple variable
substitution (`simple/` and `multi-variable/` patterns)?

If the answer is "almost all of them", option (a) with a fallback to explicit user
config is viable. If conditionals/pipes/range are common, option (b) is the safer bet.
