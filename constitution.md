# AI Development Constitution

Guidelines for using AI to build tools, automations, and applications within the company.

## 1. Purpose & Philosophy

This document defines how AI-assisted development works here. It is intentionally **permissive**: we would rather see what people actually build and guide it, than restrict up front and guess. Rules will evolve as we learn from real usage — expect this document to become more specific over time, not less.

IT is a **partner, not a gatekeeper**. There are exactly two human checkpoints, both handled through pull requests:

1. **The SPEC** — the first merge to `main` of every project is its specification, reviewed by IT.
2. **The deployment** — nothing is published or distributed except code that arrived on `main` via a reviewed pull request.

Everything between those two gates is up to you.

## 2. Project Lifecycle

1. **Request a repository.** Every automation script or application developed for a process — even a small, individual one — gets a repository in the company's GitHub organization (**American-Glass**). No personal GitHub accounts, no repositories outside the organization.
2. **Write the SPEC.** Before development starts, define and build the project's SPEC. The **first merge to `main` is the SPEC approval**, done via pull request reviewed by IT — that review is where direction, architecture, and data sources get aligned. You don't need to involve IT while writing it, but they are available to consult if you want direction earlier.
   - If the project has costs and needs budget, **leadership approval of that budget is required at SPEC approval time**.
   - If the project would require a new license or external service (section 8), that need must be raised in the SPEC.
   - If the project deviates from the recommended stack (section 9), justify the choice in the SPEC.
   - If the project processes **personal data** (customers, employees, etc.), state it in the SPEC — this matters for LGPD compliance.
3. **Develop on branches.** Work happens on branches and reaches `main` through pull requests.
4. **Deploy / distribute.** Only code merged to `main` through a reviewed PR gets deployed or distributed. How that happens depends on the project type:
   - **Web applications** — anything accessed through a browser that isn't a standalone HTML file — are deployed by IT using **Coolify** and must therefore include a **Dockerfile**.
   - **Everything else** (executables, scripts, standalone dashboards) needs no Dockerfile; its distribution method is whatever the SPEC defines.

**"Works on my PC" is not a deployment plan.** Projects that genuinely live on a single machine (e.g. a personal executable with no external data dependencies) are perfectly fine. What is not accepted is a project that needs to be distributed or accessed by others but was only ever validated locally — relying on files, data sources, or permissions that exist on the developer's machine and break the moment someone else runs it. If a project needs distribution or depends on shared data, that access (permissions, app registrations, reaching the live source rather than a local copy) is part of the architecture and belongs in the SPEC.

## 3. What a SPEC Contains — 🚧 under construction

> This section is not final. The minimum contents of a SPEC are still being defined and reviewed. The list below is a **draft starting point for that discussion**, not a rule yet.

A SPEC should at least answer:

- What problem does the project solve, and for whom?
- Who will use it, and roughly how many people?
- What data sources does it need? (see section 5)
- How will it be distributed or deployed?
- Does it have costs, need budget, or require a new license or external service?
- Does it process personal data?

## 4. Data Storage

- **Do not use external storage services** (e.g. Supabase, Firebase, or similar).
- All databases are provided by IT through:
  - **ProjetosADM** — for approved processes (production)
  - **ProjetosADM_DEV** — for development
- **Database credentials are per project and must be requested from IT.** IT creates a dedicated database user for each project/application on the ProjetosADM bases. Applications authenticate with their own project credentials — never with personal logins, and never with credentials borrowed from another project.
- **SQLite must not live in local folders.** SQLite files are only kept on the shared server folders (`T:` drive — ask IT for the server address).

## 5. Approved Data Origins

Application data must come only from:

- The Azure SQL Server databases (**ProjetosADM** / **ProjetosADM_DEV**)
- Shared files on the server folder (`T:` drive) — e.g. Excel, SQLite, JSON
- `.json` files published on an accessible site
- External API queries
- Company databases (BIAGP, DF_AGPB, SAGA, etc.)

## 6. Version Control

- Every repository must have a `.gitignore` filtering out documents and binaries: Word, Excel, PDF, DXF, DWG, `.zip`, `.rar`, and development cache files.
- Every repository must have a **README** explaining what the project does and how to run it — well enough that someone other than the author could take it over.
- All projects live exclusively in the **American-Glass** organization. No publishing to personal GitHub accounts or repositories outside the organization.

## 7. Authentication

- Web applications that require authentication must use **MSAUTH** (Microsoft authentication).

## 8. Licenses & External Services

**No new licenses or external services will be acquired**, except in cases where there is genuinely no other option. If a project seems to need one, raise it in the SPEC — the need is evaluated there, alongside the budget approval when costs are involved.

## 9. Recommended Stack

This stack should cover the vast majority of applications. It is the **default**: you may use something else, but the deviation must be justified in the SPEC so IT can weigh in during review. Exceptions are expected for cases where performance is critical or a genuine blocker (e.g. genetic algorithms).

| Project type | Recommendation |
|---|---|
| Web frontend | React + Vite |
| Web backend | Python with FastAPI, `pyodbc` (via ODBC Driver 17 for SQL Server), SQLAlchemy |
| Native GUI | Python with PyQt or tkinter |
| Terminal / CLI | Python with `.venv` for dependencies; PyInstaller to generate the `.exe` for distribution |
| Dashboards | HTML with JSON on the shared folders, or Streamlit |

## 10. Recommended Ways of Working

Like the stack, these are **recommendations, not requirements**. But if you're building with AI and don't have a methodology of your own, start here — they cost little and pay for themselves quickly:

- **Spec-driven development.** The SPEC being your first merge isn't just a gate — it works as a method. Before each feature, write down what it should do and how you'll know it works, and give that to your AI tool as the task. AI produces far better code from a clear spec than from a vague prompt, and the spec doubles as documentation.
- **Test-driven development (TDD).** Write the test first, watch it fail, implement, watch it pass. This matters *more* with AI, not less: tests are how you verify code you didn't write line by line, and a test that failed before the implementation is proof it actually tests something.
- **Small steps, small PRs.** Ask for one change at a time and commit working states often. If something breaks, you know exactly which step broke it — and small PRs get reviewed faster.
- **Understand what you accept.** If the AI generated code you can't explain, ask it to explain before you merge. Your area maintains this project (section 12); future-you is the audience.

## 11. Security & Data Hygiene (AI baseline)

When using AI to develop, these apply regardless of how permissive everything else is:

- **Never commit secrets.** Passwords, tokens, API keys, and connection strings do not go in code, documentation, or Git history. Use environment variables; `.env` files belong in `.gitignore`.
- **Warn before destructive operations.** Deleting data in bulk, overwriting shared files, force-pushing, or deleting repositories requires an explicit heads-up and human confirmation — never silently executed.
- **Prefer development data.** Build and test against ProjetosADM_DEV or sample data; touch production data only when necessary and deliberately.
- **Don't fabricate.** Test results, data, and claims of "it works" must be backed by evidence, not invented to make something look finished.

## 12. Maintenance & QA

The area that developed a project holds **primary responsibility for maintaining it** and guaranteeing it works well — data quality, file processing, and overall application health. IT deploys and supports the platform; the owning team owns the application.

## 13. Evolving This Document

This constitution is a starting point, calibrated to be permissive while we learn. Planned improvements include a query/API environment provided by IT for data access, avoiding direct connections to internal databases. Suggestions and change requests are welcome — via pull request, naturally.
