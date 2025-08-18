# Naming Taxonomy (Repos, Packages, Artifacts)

## 1) Scope prefixes (no collisions)

* **`pr_`** → *Project-scoped deployables* (apps/services belonging to one project)
* **`pf_`** → *Platform (org-wide) deployables* (shared services/infra used by many projects)
* **`pkg_`** → *Reusable packages* (libraries/SDKs/components — not directly deployable)

> Reserve these prefixes. Do **not** use them as project names or folder names.

---

## 2) Canonical patterns

### Project-scoped (deployable apps)

```
pr_{project}_app_{runtime}
```

* `project`: short slug (lowercase, underscores only if compound), e.g. `mjedisi`, `platform`, `akm`
* `runtime`: app type or runtime: `api`, `web`, `console`, `front-react`, `worker`, `gateway`

**Examples**

```
pr_mjedisi_app_api
pr_mjedisi_app_console
pr_platform_app_front-react
```

---

### Platform (org-wide deployable apps)

```
pf_{domain}_app_{runtime}
```

* `domain`: business/tech area: `user`, `auth`, `notify`, `files`, `ui`, `devops`
* `runtime`: runtime kind or role: `api`, `gateway`, `shell`, `scheduler`, `front-react`

**Examples**

```
pf_user_app_api
pf_auth_app_gateway
pf_ui_app_shell
pf_notify_app_scheduler
```

---

### Reusable packages (non-deployable)

```
pkg_{domain}_{name}[_{lang|stack}]
```

* Optional language/stack suffix when useful: `ts`, `py`, `cs`, `java`, `react`, `nest`

**Examples**

```
pkg_ui_components_react
pkg_user_sdk_ts
pkg_auth_client_cs
pkg_core_logging
```

---

## 3) Quick decision tree

1. **Is it directly deployable?**

   * **Yes** → use `*_app_*`
   * **No** → use `pkg_{domain}_{name}`

2. **Does it serve multiple projects?**

   * **Yes** → `pf_{domain}_app_{runtime}`
   * **No** → `pr_{project}_app_{runtime}`

---

## 4) Do / Don’t

**Do**

* Always include `_app_` for deployables.
* Use **underscores** for separation, **hyphens only inside runtime/stack suffix** (`front-react`).
* Keep runtime tokens **short & semantic** (`api`, `web`, `console`, `worker`).
* Use registry scopes for published code packages.

**Don’t**

* Don’t mix underscores and hyphens except inside stack identifiers.
* Don’t append environment (`_dev`, `_prod`) to repo names — use CI/branches.

---

## 5) Regex checkers

```regex
# project deployables
^pr_[a-z0-9]+(_[a-z0-9]+)*_app_(api|web|console|front-react|worker|gateway|shell|scheduler)$

# platform deployables
^pf_[a-z0-9]+(_[a-z0-9]+)*_app_(api|web|console|front-react|worker|gateway|shell|scheduler)$

# packages
^pkg_[a-z0-9]+(_[a-z0-9]+)*(_[a-z0-9]+)*(_(ts|py|cs|java|react|nest))?$
```

---

## 6) Common examples (matrix)

| Type                  | Example                       | Notes                    |
| --------------------- | ----------------------------- | ------------------------ |
| Project API           | `pr_mjedisi_app_api`          | Project backend service  |
| Project Console       | `pr_mjedisi_app_console`      | SPA/SSR front            |
| Project React Front   | `pr_platform_app_front-react` | Explicit React runtime   |
| Platform User API     | `pf_user_app_api`             | Shared authn/profile     |
| Platform Auth Gateway | `pf_auth_app_gateway`         | Reverse proxy/OPA        |
| Platform UI Shell     | `pf_ui_app_shell`             | Micro-frontend host      |
| Platform Scheduler    | `pf_notify_app_scheduler`     | Worker/scheduler process |
| UI Components         | `pkg_ui_components_react`     | Reusable React lib       |
| User SDK (TS)         | `pkg_user_sdk_ts`             | Published to npm         |
| Logging Core          | `pkg_core_logging`            | Lang-agnostic design     |

---

## 10) Migration guide (from your old scheme)

| Old                      | New (aligned `_app_`)              |
| ------------------------ | ---------------------------------- |
| `pr_mjedisi_app_api`     | `pr_mjedisi_app_api` (unchanged ✅) |
| `pr_mjedisi_app_console` | `pr_mjedisi_app_console`           |
| Global user service      | `pf_user_app_api`                  |
| Global UI components     | `pkg_ui_components_react`          |
| Global user SDK (TS)     | `pkg_user_sdk_ts`                  |
