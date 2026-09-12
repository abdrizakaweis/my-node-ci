# my-node-ci

A minimal Node.js application with a GitHub Actions continuous integration pipeline.

## What the pipeline does

| Job | Trigger | What it does |
|---|---|---|
| Test | Push + PR to `main` | `npm ci`, lint, `npm test` |
| Build | After Test passes | `npm run build`, uploads `dist/` as an artifact |

Both jobs are required status checks on `main`, so a failed pipeline blocks the merge.

## Running locally

```bash
npm ci
npm test
npm run build
```

## Pipeline decisions

- **`npm ci`, not `npm install`** — installs strictly from the lockfile, making CI builds reproducible.
- **Build is a separate job** — it only runs after tests pass, so a broken commit does not produce a build artifact.
- **Concurrency with `cancel-in-progress`** — newer pushes cancel older unfinished runs on the same branch.
- **`permissions: contents: read`** — the workflow only needs read access to the repository.

## Possible improvements

- Add a real linter such as ESLint instead of the placeholder lint script.
- Add a test coverage threshold that fails the pipeline when coverage is too low.
- Add a matrix to test multiple Node.js versions such as Node 18, 20, and 22.