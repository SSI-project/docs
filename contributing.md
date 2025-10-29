# Contributing to PortableID

Thank you for contributing! We welcome all contributions—bug reports, feature requests, code, and documentation.

## Code of Conduct

We are committed to providing a welcoming and inclusive environment. Please read and follow our [Code of Conduct](./CODE_OF_CONDUCT.md).

## Getting Started

### 1. Fork & Clone

bash
git clone https://github.com/SSI-project/[REPO_NAME].git
cd [REPO_NAME]
npm install  # or appropriate package manager


### 2. Create a Branch

bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/issue-number


Branch naming convention: `feature/`, `fix/`, `docs/`, `refactor/`, `test/`

### 3. Make Changes

- Follow the existing code style (ESLint, Prettier configured)
- Write tests for new features
- Update documentation if needed

### 4. Test Locally

bash
npm test
npm run lint
npm run build


### 5. Commit with Conventional Commits

bash
git add .
git commit -m "feat: add new feature description"
# or
git commit -m "fix: resolve issue #123"
git commit -m "docs: update README"


Commit types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`, `perf`

### 6. Push & Open PR

bash
git push origin feature/your-feature-name


Open a Pull Request on GitHub with:
- Clear title describing the change
- Reference to related issues (#123)
- Explanation of what changed and why
- Screenshots/videos if UI changes

## PR Review Process

1. At least one maintainer review required
2. All CI/CD checks must pass
3. Tests must have >80% coverage
4. No unresolved conversations

## Code Style

### JavaScript/TypeScript
- Use ESLint & Prettier (auto-format on save)
- 2-space indentation
- Named exports preferred
- Add JSDoc comments for public functions

javascript
/**
 * Issues a verifiable credential.
 * @param {Object} issuer - The issuer object
 * @param {string} subject - The subject DID
 * @param {Object} claims - The credential claims
 * @returns {Promise} The issued credential
 */
export async function issueCredential(issuer, subject, claims) {
  // implementation
}


### Rust (Smart Contracts)
- Use \`cargo fmt\` and \`cargo clippy\`
- Follow Substrate best practices
- Add extensive tests in \`#[cfg(test)]\` module

### Documentation
- Clear, concise English
- Include examples where helpful
- Update related docs when changing features

## Testing

All contributions must include tests:

bash
# Unit tests
npm run test:unit

# Integration tests
npm run test:integration

# Coverage report
npm run test:coverage


Target: >80% code coverage

## Reporting Issues

Found a bug? [Open an issue](https://github.com/SSI-project/[REPO_NAME]/issues/new/choose) with:

- **Title**: Clear, descriptive title
- **Description**: What you expected vs. what happened
- **Steps to Reproduce**: Exact steps to recreate
- **Environment**: OS, Node version, browser, etc.
- **Screenshots/Logs**: If applicable

### Security Issues

**DO NOT** open public issues for security vulnerabilities.
Email security@portableid.dev or see [SECURITY.md](./SECURITY.md).

## Documentation Updates

- Update [docs/](./docs/) folder for architectural changes
- Update [README.md](./README.md) for setup/usage changes
- Add comments for complex logic
- Keep examples up-to-date

## Deployment Changes

Changes affecting production need:
- [ ] Updated deployment docs
- [ ] Migration scripts (if DB changes)
- [ ] Rollback plan
- [ ] Monitoring/alerting configured
- [ ] Security review

## Project Structure

See [README.md](./README.md#project-structure) for folder layout.

## Questions?

- **GitHub Issues**: [Ask a question](https://github.com/SSI-project/[REPO_NAME]/issues/new/choose)
- **Discussions**: [GitHub Discussions](https://github.com/SSI-project/[REPO_NAME]/discussions)
- **Documentation**: [PortableID Docs](https://docs.portableid.dev)

## License

By contributing, you agree that your contributions will be licensed under the same MIT License as the project.

Thank you for making PortableID better! 🚀
