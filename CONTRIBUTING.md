# Contributing to Trustless Hiring Marketplace

Please read this first: **this project does not accept code contributions.**

It is a proof-of-concept and portfolio piece exploring trustless hiring with
zero-knowledge identity verification. The source is published for viewing and
evaluation under a proprietary licence (see [LICENSE](LICENSE)), which does not
grant permission to copy, fork, or modify the code. Pull requests therefore
cannot be merged, and forking falls outside the terms of use.

**Issues, questions, and feedback are genuinely welcome**, and are the useful
way to engage with this project.

## 📋 What is welcome

### Reporting issues

- Use GitHub Issues with clear titles and descriptions
- Include reproduction steps for bugs
- Label issues appropriately (`bug`, `enhancement`, `documentation`, `research`)

### Suggesting enhancements

- Open a GitHub Issue with the `enhancement` label
- Describe the use case and expected behavior
- Reference any relevant research or prior art

### Reporting security problems

- Report vulnerabilities **privately** via GitHub Security Advisories, not as
  public issues
- Never include secrets, API keys, or credentials in a report

## 🚫 What cannot be accepted

- **Pull requests.** They will be closed unmerged regardless of quality, because
  the licence does not permit modification.
- **Forks.** The licence does not grant permission to copy or redistribute.

If you want to build on these ideas, please open an issue to ask about
permission rather than forking.

## 🏗️ Project philosophy

This project values:

1. **Privacy First** — Every feature should default to minimal data exposure
2. **Documented Decisions** — Every architectural choice should be recorded with rationale
3. **Incremental Evolution** — Build in phases, validate assumptions, iterate
4. **Portfolio Quality** — Code and documentation should be professional and well-crafted

## 📐 Project conventions

Recorded for reference and for anyone evaluating this project, not as
instructions for outside contributors.

### Commit messages

Following [Conventional Commits](https://www.conventionalcommits.org/):

| Prefix | Usage |
|--------|-------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation changes |
| `refactor:` | Code refactoring (no feature change) |
| `test:` | Adding or updating tests |
| `chore:` | Build process or tooling changes |
| `research:` | Research findings or protocol analysis |

### Code style

- **JavaScript/TypeScript**: ESLint + Prettier (configured in project)
- **SQL**: Uppercase keywords, lowercase identifiers, clear aliasing
- **Documentation**: Markdown with consistent heading hierarchy
- **Comments**: Explain _why_, not _what_ — the code should explain what

### Security practices

- **Never commit secrets, API keys, or credentials**
- Use `.env` files for all configuration (see `.env.example`)
- All ZK-related code should be reviewed for cryptographic correctness

## 📄 License

This project is **proprietary and source-available**, not open source. See
[LICENSE](LICENSE) for the full terms.

Copyright (c) 2026 Mark Pommrehn. All rights reserved.

---

_Questions? Open an issue._
