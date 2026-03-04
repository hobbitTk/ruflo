# Contributing

## Setup

```bash
git clone https://github.com/hobbitTk/ruflo.git
cd ruflo
```

### Build the CLI

```bash
cd v3/@claude-flow/cli
npm install
npm run build
```

### Run Tests

```bash
# From repo root
npm test

# Or from v3/
cd v3
pnpm test
```

## Making Changes

1. Create a branch from `main`
2. Make your changes in `v3/@claude-flow/` packages
3. Add tests for new functionality
4. Ensure `npm test` passes
5. Open a PR with a clear description

## Code Standards

- TypeScript with strict mode
- No `any` types in public APIs
- Input validation at system boundaries (use `@claude-flow/security`)
- Files under 500 lines
- Use `execFile`/`spawn` with argument arrays, never `exec` with string interpolation

## Security

If you find a security vulnerability, please open a GitHub issue. For the upstream project, also reference the relevant issue at [ruvnet/ruflo](https://github.com/ruvnet/ruflo/issues).
