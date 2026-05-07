# Frontend Builds At Monorepo Scale With Bazel

This is a generic series outline for frontend build systems in large monorepos. It intentionally avoids company-specific package names, repository paths, service names, and internal implementation details.

## 1. Why Bazel For Large Frontend Monorepos?

### Abstract

Introduce the core problem: a large frontend monorepo is not just a set of package scripts. It is a graph of apps, shared packages, generated clients, assets, tests, verification steps, and deployment artifacts. Bazel is framed as a way to model that graph precisely enough for reliable selective builds.

### Covers

- Why package scripts stop scaling
- Selective builds as a correctness problem
- Explicit source, asset, config, test, and deployment boundaries
- Generated clients, translation catalogs, icon sprites, and route metadata as graph artifacts
- Post-build verification and deployment targets as consumers of built artifacts
- Selective side effects tied to changed artifacts
- Why ergonomics matter as much as graph precision

## 2. The Anatomy Of A Frontend Package

### Abstract

Explain what a frontend package means in a Bazel-modeled monorepo, then connect that package shape to generated BUILD metadata and composable rule kinds. This post combines package anatomy with the generation story because package conventions are what make codegen useful.

### Covers

- Internal packages vs leaf packages
- Why creating a new `package.json` for every boundary is not always ergonomic
- Runtime source, tests, stories, configs, assets, and generated outputs
- Companion targets and stable target names
- Generated BUILD metadata with tools such as `hermeticbuild/gazelle_ts`
- Absolute subpath imports for refactor-friendly dependency resolution
- What generators should infer vs what humans should decide
- Package splitting ergonomics

## 3. Typechecking And Dependency Hygiene

### Abstract

Treat typechecking and dependency declarations as two sides of the same contract. Runtime source, tests, config files, generated clients, browser code, and server code need different type and dependency surfaces.

### Covers

- Typecheck vs transpilation
- Typechecking performance vs faster transpilation/parsing tools such as `tsgo` and Oxc
- Runtime, test, config, generated, browser, and server contracts
- Build tsconfig vs editor tsconfig
- Package-manager deps vs build deps
- Test-only, config-only, ambient, and generated deps
- Visibility and layering checks
- Circular dependency prevention
- Go-style `internal` boundaries for frontend packages
- Why lint rules and lint fixtures also need typecheck

## 4. Transitive Metadata And Generated Packages

### Abstract

Show how Bazel can model artifacts that are neither ordinary handwritten source nor final bundles: translations, icon sprites, route metadata, internal dependency reports, generated REST clients, protobuf declarations, and GraphQL operation packages.

### Covers

- Aspects for graph-aware metadata collection
- Translation extraction with FormatJS-style colocated messages and app-level aggregation
- Icon sprite extraction
- Route metadata aggregation across apps to detect collisions
- Internal dependency metadata
- Generated REST, protobuf, and GraphQL packages
- Generated-import conventions
- Direct dependencies on generated targets

## 5. Apps, SDKs, And Tests In The Build Graph

### Abstract

Explain how leaf app and SDK targets compose package primitives, and how tests consume those same package surfaces. Vite remains the bundler; Bazel models its inputs, outputs, and downstream consumers.

### Covers

- Apps and SDKs as leaf nodes
- Typed Vite config targets
- App roots that typecheck without emitting JavaScript
- Runtime assets and environment inputs
- Devserver targets
- Unit tests, test typecheck, snapshots, Storybook, visual regression, fixtures, and coverage

## 6. Verifying And Deploying Frontend Artifacts

### Abstract

Move beyond `dist`. This post treats built frontend artifacts as APIs that need verification and as inputs to deployment targets such as server bundles, OCI images, CDN uploads, browser extensions, and edge workers.

### Covers

- Output verification
- File-size checks
- Environment replacement checks
- CDN and sourcemap paths
- Built asset checks for forbidden strings, sensitive constants, chunk placement, CSS loading, and runtime CSS invariants
- Upload checks for public assets, sourcemaps, restricted debug artifacts, success markers, and signing-dependent packages
- Generated asset validation
- Server bundles and frontend images
- Static uploads, browser extensions, edge workers, stamping, and preview deploys
- Selective side effects: upload/deploy only when the consumed artifact changed

## 7. Bazel vs Turborepo vs Nx

### Abstract

Go deep on Bazel, Turborepo, and Nx through the lens of large frontend monorepos. This is not a universal ranking; it is a comparison of models: package task orchestration, project graph tooling, and explicit artifact/action graphs.

### Covers

- Turborepo: package task orchestration
- Nx: project graph and workspace tooling
- Bazel: explicit action/artifact graph
- When each tool fits
- Frontend-specific decision points
- Caching granularity
- Generated artifacts and custom graph analysis
- Developer experience tradeoffs

## 8. Operating The Build System As Product Infrastructure

### Abstract

Close by discussing what it takes to keep a Bazel-based frontend build system useful after the migration is over.

### Covers

- Build rules as platform APIs
- Documentation, errors, metrics, deletion, self-service, and gradual strictness
