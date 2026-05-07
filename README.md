# Thoughts

Long-form technical writing drafts and outlines.

This repository is a place to develop essays, blog post series, and notes about software engineering systems. The writing here is intended to be public-facing: examples should be generic, company-neutral, and free of private repository names, internal service names, or unpublished implementation details.

## Current Series

### Frontend Builds At Monorepo Scale With Bazel

Drafts for a series about using Bazel to model frontend build systems in large monorepos.

The series covers:

- why package-level task runners stop being enough for some frontend graphs
- ergonomic frontend package boundaries
- generated BUILD metadata
- strict typechecking and dependency hygiene
- transitive metadata collection with aspects
- generated TypeScript packages
- app, SDK, and test targets
- built artifact verification
- deployment targets and selective side effects
- tradeoffs between Bazel, Turborepo, and Nx

Start with [the outline](frontend-bazel-monorepo-series/outline.md), then read the posts in numeric order.
