# Naming Taxonomy (Repos, Packages, Artifacts)

## 1) Scope prefixes (no collisions)

* **`pr_`** → *Project-scoped deployables* (apps/services belonging to one project)
* **`pf_`** → *Platform (org-wide) deployables* (shared services/infra used by many projects)
* **`pkg_`** → *Reusable packages* (libraries/SDKs/components — not directly deployable)

> Reserve these prefixes. Do **not** use them as project names or folder names.

---

## 2) Canonical patterns

### Project-scoped (deployable)

```
pr_{project}-{app}
```

* `project`: short slug (lowercase kebab), e.g. `mjedisi`, `platform`, `akm`
* `app`: runtime type or name: `api`, `web`, `worker`, `console`, `gateway`

**Examples**

```
pr_mjedisi-api
pr_mjedisi-console
pr_platform-worker
```

### Platform (org-wide) deployables

```
pf_{domain}-{service}
```

* `domain`: business/tech area: `user`, `auth`, `notify`, `files`, `ui`, `devops`
* `service`: runtime kind or role: `api`, `gateway`, `scheduler`, `shell`, `registry`

**Examples**

```
pf_user-api
pf_auth-gateway
pf_ui-shell
pf_notify-scheduler
```

### Reusable packages (non-deployable)

```
pkg_{domain}-{name}[-{lang|stack}]
```

* Optional language/stack suffix when useful: `ts`, `py`, `cs`, `java`, `react`, `nest`

**Examples**

```
pkg_ui-components-react
pkg_user-sdk-ts
pkg_auth-client-cs
pkg_core-logging
```

---

## 3) Quick decision tree

1. **Is it directly deployable?**

   * **Yes** → Go to 2
   * **No** → `pkg_{domain}-{name}[-{lang}]`

2. **Used by multiple projects?**

   * **Yes** → `pf_{domain}-{service}`
   * **No, belongs to one project** → `pr_{project}-{app}`

---

## 4) Do / Don’t

**Do**

* Use **lowercase kebab-case** for all tokens after the prefix.
* Keep names **short & semantic** (`api`, `web`, `worker`, `sdk`, `components`).
* Add **registry scopes** for code packages (npm, PyPI, NuGet) — see §7.

**Don’t**

* Don’t mix underscores after the prefix (stick to kebab afterwards).
* Don’t use reserved prefixes (`pr_`, `pf_`, `pkg_`) as project names.
* Don’t encode environment (`-dev`, `-prod`) in repo names — use branches/CI.

---

## 5) Regex checkers (for a pre-commit hook)

```regex
# project deployables
^pr_[a-z0-9]+(-[a-z0-9]+)*-(api|web|worker|console|gateway|shell|scheduler)$

# platform deployables
^pf_[a-z0-9]+(-[a-z0-9]+)*-(api|web|worker|gateway|shell|scheduler|registry)$

# packages
^pkg_[a-z0-9]+(-[a-z0-9]+)*-[a-z0-9]+(-[a-z0-9]+)*$
```

---

## 6) Common examples (matrix)

| Type                  | Example                   | Notes                  |
| --------------------- | ------------------------- | ---------------------- |
| Project API           | `pr_mjedisi-api`          | Single-project backend |
| Project Web           | `pr_mjedisi-web`          | SPA/SSR                |
| Platform User API     | `pf_user-api`             | Shared authn/profile   |
| Platform Auth Gateway | `pf_auth-gateway`         | Reverse proxy/OPA      |
| Platform UI Shell     | `pf_ui-shell`             | Micro-frontend host    |
| UI Components         | `pkg_ui-components-react` | Reusable React lib     |
| User SDK (TS)         | `pkg_user-sdk-ts`         | Published to npm       |
| Logging Core          | `pkg_core-logging`        | Lang-agnostic design   |

---

## 7) Package publishing (by ecosystem)

* **npm**: `@org/{domain}-{name}`

  * Repo: `pkg_user-sdk-ts` → Package: `@org/user-sdk`
* **PyPI**: `org-{domain}-{name}`

  * `org-user-sdk`
* **NuGet**: `Org.{Domain}.{Name}`

  * `Org.User.Sdk`
* **Docker**: `ghcr.io/org/{repo}:{tag}`

  * `ghcr.io/org/pf_user-api:1.4.0`

> Keep repo **name** stable; adapt published **package** name to ecosystem norms.

---

## 8) Tags, branches, and topics

* **Tags**: SemVer (`v1.4.0`). For mono-pkg repos, plain tags; for multi-pkg (rare here), use Changesets.
* **Branches**: `main` (protected), feature branches `feat/*`, `fix/*`.
* **Topics** (GitHub): `scope:project|platform|package`, `domain:user|ui|auth`, `type:api|sdk|components`.

---

## 9) README header template (copy/paste)

```
# pf_user-api

**Scope**: platform (org-wide)  
**Domain**: user  
**Type**: api (deployable)  

- Contract: `openapi/openapi.yaml`
- Image: `ghcr.io/<org>/pf_user-api`
- Envs: `/deploy/helm/values*.yaml`
- Changelog: `CHANGELOG.md`
```

(Adjust for `pr_*` and `pkg_*` accordingly.)

---

## 10) Migration guide (from your current scheme)

| Old                      | New                       |
| ------------------------ | ------------------------- |
| `pr_mjedisi_app_api`     | `pr_mjedisi-api`          |
| `pr_mjedisi_app_console` | `pr_mjedisi-console`      |
| Global user service      | `pf_user-api`             |
| Global UI components     | `pkg_ui-components-react` |
| Global user SDK (TS)     | `pkg_user-sdk-ts`         |

**CI tip:** introduce a `repo-map.json` to keep pipelines working during the rename window.

---

## 11) Enforcement aids

* Add a simple **lint GitHub Action** that checks repo name against §5 regex.
* Provide a `bin/create-repo` script that scaffolds README, topics, branch protections using the chosen pattern.

---

If you want, I can generate:

* a ready-to-use **GitHub Action** for name linting, and
* a tiny **CLI scaffold** (`create-repo.mjs`) that enforces this taxonomy interactively.

