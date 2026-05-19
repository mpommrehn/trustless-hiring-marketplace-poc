# Contributing to Trustless Hiring Marketplace

Thank you for your interest in contributing! This project is a proof-of-concept exploring trustless hiring with zero-knowledge identity verification. Contributions of all kinds are welcome.

## 🏗️ Project Philosophy

This project values:

1. **Privacy First** — Every feature should default to minimal data exposure
2. **Documented Decisions** — Every architectural choice should be recorded with rationale
3. **Incremental Evolution** — Build in phases, validate assumptions, iterate
4. **Portfolio Quality** — Code and documentation should be professional and well-crafted

## 📋 How to Contribute

### Reporting Issues

- Use GitHub Issues with clear titles and descriptions
- Include reproduction steps for bugs
- Label issues appropriately (`bug`, `enhancement`, `documentation`, `research`)

### Suggesting Enhancements

- Open a GitHub Issue with the `enhancement` label
- Describe the use case and expected behavior
- Reference any relevant research or prior art

### Code Contributions

1. **Fork** the repository
2. **Create a feature branch** from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes** following the code style guidelines below
4. **Write or update tests** for your changes
5. **Update documentation** if your changes affect the API or architecture
6. **Commit** with clear, descriptive messages:
   ```bash
   git commit -m "feat: add weighted matching for culture fit criteria"
   ```
7. **Push** to your fork and open a **Pull Request**

### Commit Message Convention

We follow [Conventional Commits](https://www.conventionalcommits.org/):

| Prefix | Usage |
|--------|-------|
| `feat:` | New feature |
| `fix:` | Bug fix |
| `docs:` | Documentation changes |
| `refactor:` | Code refactoring (no feature change) |
| `test:` | Adding or updating tests |
| `chore:` | Build process or tooling changes |
| `research:` | Research findings or protocol analysis |

### Code Style

- **JavaScript/TypeScript**: ESLint + Prettier (configured in project)
- **SQL**: Uppercase keywords, lowercase identifiers, clear aliasing
- **Documentation**: Markdown with consistent heading hierarchy
- **Comments**: Explain _why_, not _what_ — the code should explain what

## 🔒 Security

- **Never commit secrets, API keys, or credentials**
- Use `.env` files for all configuration (see `.env.example`)
- Report security vulnerabilities privately via GitHub Security Advisories
- All ZK-related code should be reviewed for cryptographic correctness

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

_Questions? Open an issue or start a discussion. We're building this in the open._
