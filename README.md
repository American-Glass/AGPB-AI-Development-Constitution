# AGPB AI Development Constitution

The constitution for AI-assisted development of tools, automations, and applications at AGP Brazil.

📜 **The document: [constitution.md](constitution.md)**

## What this is

A deliberately lean set of rules for building software with AI inside the company. It is **permissive by design**: instead of restricting up front, it defines exactly **two human checkpoints** — the SPEC as the first merge to `main`, and deployment only through reviewed pull requests — and leaves everything in between to the developer.

The rules will evolve based on what people actually build, so expect this document to become more specific over time, not less.

## How to use it

- **If you're building something:** read [constitution.md](constitution.md) before you start. It tells you how to get a repository, what the SPEC gate is, where your data and databases come from, and the recommended stack that covers most use cases.
- **If you're using an AI coding tool:** give it `constitution.md` as context (e.g. reference it from your project's instructions file). The rules are written to be followed by AI agents as well as humans.

## Quick summary

1. Every project gets a repository in the **American-Glass** GitHub organization.
2. The first merge to `main` is the project's **SPEC**, reviewed by IT via pull request.
3. Development happens on branches; everything reaches `main` through PRs.
4. Web apps are deployed by IT via Coolify (Dockerfile required); other projects distribute as defined in their SPEC.
5. Data lives in IT-provided databases and approved sources — no external storage services.

The full rules, including storage, authentication, licensing, security baseline, and the recommended stack, are in [constitution.md](constitution.md).

## Status

This is a living document — some sections are explicitly marked as under construction. Suggestions and change requests are welcome via pull request.
