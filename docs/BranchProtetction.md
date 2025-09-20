# DevOps Task

## Branching Strategy

This project follows a strict branching strategy to ensure code quality and controlled deployments.

### Branch Naming Conventions
- `main`: Production environment (protected)
- `staging`: Staging/Pre-production environment (protected)
- `dev`: Development environment (protected)
- `feature-<feature-name>/<name>`: Feature branches (e.g., `feature-auth/user-authentication`)

### Branch Protection Rules
1. **Direct Pushes** to `dev`, `staging`, and `main` are **not allowed**
2. **Pull Request Requirements**:
   - All PRs require at least one approval
   - All status checks must pass before merging
   - Branches must be up to date before merging

### Hotfix Exception (Main)
- Only commits authored/pushed by `thulanibango@gmail.com` may be pushed directly to `main`.
- This is enforced in CI via `.github/workflows/branch-protection.yml` and should be paired with GitHub Branch Protection:
  - In `Settings` → `Branches` → `main` rule, enable "Require a pull request before merging".
  - Add an exception: "Allow specified actors to bypass required pull requests" and select the `thulanibango` user. This ensures only that account can bypass PRs for hotfix pushes to `main`.
  - Keep direct pushes blocked for everyone else.

### Merge Strategy
1. **Feature Branches → dev**:
   - Must follow naming pattern: `feature-<feature-name>/<name>`
   - Requires at least one approval
   - All CI checks must pass
   - After merge to `dev`, the feature branch is automatically deleted by workflow `.github/workflows/auto-delete-feature-branches.yml` (for same-repo branches only)

2. **dev → staging**:
   - Only `dev` can be merged into `staging`
   - Requires PR with approval
   - All CI/CD checks must pass

3. **staging → main**:
   - Only `staging` can be merged into `main` (production)
   - Requires PR with approval
   - All CI/CD checks must pass
   - Deployment to production is automated after merge

> Note: As an alternative, you can enable the GitHub repository setting "Automatically delete head branches" under `Settings` → `General` → `Pull Requests`. The workflow here provides explicit control and naming checks.

### Creating a New Feature
1. Create a new branch from `dev`: `git checkout -b feature-<feature-name>/<name> dev`
2. Make your changes and commit
3. Push your branch: `git push origin feature-<feature-name>/<name>`
4. Create a pull request to `dev`
5. After approval and merge, the changes will flow through the environments

