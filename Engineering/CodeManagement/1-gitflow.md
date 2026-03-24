### Branches created by developers

`feature` | `feat`

- For new functionality

- Branches off of `develop`

- Naming: `feature/<developer_initials>/<branch_description>`
  - Example: `feature/er/new_endpoint_to_check_stock`

`bugfix`

- For new functionality

- Branches off of `develop`

- Naming: `bugfix/<developer_initials>/<branch_description>`

  - Example: `bugfix/er/add_missing_api_config`

`refactor`

- For new functionality

- Branches off of `develop`

- Naming: `refactor/<developer_initials>/<branch_description>`

  - Example: `refactor/er/change_api_service`

`hotfix`

- For fast patches to production

- Branches off of `master`

- Naming: `hotfix/<developer_initials>/<branch_description>`

  - Example: `hotfix/er/add_missing_api_config`

### Release branch created only by release manager.

`release`

- For preparing next version of product

- Branches off of `develop`

- Naming: `release/<version>`

  - Example: `release/v1.0`

### Base branches

`main`

- Represents our project that is live.

- Our “production ready” code

- Lives forever

`develop`

- `develop` (or we can call it the integration branch) .It is first created after `main` and after creating it, it never has to interact with `main` again. `develop` is where we create our branches off.

- Your “working” code

- Lives forever

### Commit conventions

- use same convetions also for messages

  `git commit -m"feat: expain what part of the feature you did"`

  `git commit -m"fix: at least add the ':' after fix"`
