# GDB Branching Strategy

## Branch Structure

```
main
└── dev
    ├── feature/containerization
    ├── feature/cicd-pipeline
    ├── feature/kubernetes
    └── feature/<name>
```

## Branches

| Branch | Purpose | Direct Push |
|--------|---------|-------------|
| `main` | Production-ready code | No (PR only) |
| `dev` | Integration branch | No (PR only) |
| `feature/*` | New features / DevOps work | Yes |
| `hotfix/*` | Critical production fixes | No (PR to main) |

## Workflow

1. **New Work** — branch off `dev`: `git checkout -b feature/<name> dev`
2. **Development** — commit to the feature branch
3. **Pull Request** — open PR from `feature/<name>` → `dev`
4. **Review & Merge** — at least 1 approval required before merge
5. **Release** — PR from `dev` → `main` when features are stable

## Branch Protection Rules (GitHub)

### `main`
- Require pull request before merging
- Require at least 1 approving review
- Dismiss stale PR approvals when new commits are pushed
- Require status checks to pass (CI build)
- Disallow direct pushes

### `dev`
- Require pull request before merging
- Require status checks to pass (CI build)
- Disallow direct pushes

## Commit Message Convention

```
<type>(<scope>): <short description>

Types: feat | fix | docs | style | refactor | test | chore | ci
```

Examples:
- `feat(auth-service): add JWT refresh token endpoint`
- `ci(pipeline): add Docker image push to registry`
- `fix(account-service): correct aadhar service URL port`
