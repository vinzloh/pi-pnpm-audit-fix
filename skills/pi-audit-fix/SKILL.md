name: frontend-audit-fix
description: Use this skill when `pnpm audit` is executed or results requiring fixing dependencies
---

## Requirements

- run `pnpm audit` if required. if there are no vulnerabilities, stop here.
- In `pnpm-workspace.yaml`, comment out `minimumReleaseAge` line (if present)
- Identify vulnerable dependency with `pnpm why <dependency>`
- Try `pnpm audit --fix` first (automatic resolution when available)
- If not fixed, try updating parent dependency: `pnpm up <parent>@latest`
- Verify with `pnpm audit`
- If still vulnerable, add override to `package.json` under `pnpm.overrides`:

  ```json
  "pnpm": {
    "overrides": {
      "<dependency>": ">=<patched-version>"
    }
  }
  ```

- Run `pnpm install` to apply overrides
- Verify with `pnpm audit`
- Verify codebase with `pnpm lint`, `pnpm typecheck`
- Uncomment `minimumReleaseAge` in `pnpm-workspace.yaml`
- Commit the changes with a descriptive message (e.g., `git add -A && git commit -m "fix: resolve security vulnerabilities in <dependency>"`)

## Important Notes

- Some vulnerabilities may have multiple parent packages - check `pnpm why` output carefully

## Relevant files

- package.json
- pnpm-workspace.yaml
- pnpm-lock.yaml

## Commands

- `pnpm audit` - Check for vulnerabilities
- `pnpm audit --fix` - Auto-fix when available
- `pnpm why <pkg>` - Find dependency tree
- `pnpm up <pkg>@latest` - Update package

## When to Use

- Use this skill when audit results require fixing dependencies.
