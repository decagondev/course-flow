# Contributing to CourseFlow

Thank you for your interest in contributing to CourseFlow! We welcome contributions from the community to help improve this AI-powered course optimization platform. Whether you're fixing a bug, adding a new feature, improving documentation, or suggesting enhancements, your input is valuable.

This document outlines the guidelines for contributing to the project. By participating, you agree to abide by our [Code of Conduct](#code-of-conduct).

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Improving Documentation](#improving-documentation)
  - [Submitting Code Changes](#submitting-code-changes)
- [Development Setup](#development-setup)
  - [Prerequisites](#prerequisites)
  - [Cloning the Repository](#cloning-the-repository)
  - [Installing Dependencies](#installing-dependencies)
  - [Environment Configuration](#environment-configuration)
  - [Running the Project Locally](#running-the-project-locally)
- [Branching Strategy](#branching-strategy)
- [Commit Message Guidelines](#commit-message-guidelines)
- [Pull Request Process](#pull-request-process)
- [Testing Guidelines](#testing-guidelines)
- [Style Guides](#style-guides)
  - [JavaScript/TypeScript](#javascripttypescript)
  - [CSS](#css)
  - [Documentation](#documentation)
- [Review Process](#review-process)
- [Community and Support](#community-and-support)
- [Acknowledgments](#acknowledgments)

## Code of Conduct

This project adheres to the [Contributor Covenant Code of Conduct](https://www.contributor-covenant.org/version/2/0/code_of_conduct.html). By participating, you are expected to uphold this code. Please report unacceptable behavior to [decagondev@example.com](mailto:decagondev@example.com).

## How Can I Contribute?

### Reporting Bugs

If you find a bug, please check the [issues](https://github.com/decagondev/course-flow/issues) to see if it's already reported. If not:

1. Open a new issue.
2. Use the bug report template.
3. Provide details: steps to reproduce, expected vs. actual behavior, screenshots if applicable, and environment info (e.g., Node version, browser).

### Suggesting Enhancements

We love ideas! To suggest a feature or enhancement:

1. Open an issue using the feature request template.
2. Describe the problem it solves, proposed solution, and any alternatives considered.
3. Reference relevant parts of the [PRD.md](PRD.md) if applicable.

### Improving Documentation

Documentation is key. Contributions to README.md, API docs, or inline comments are welcome. Follow the same PR process as code changes.

### Submitting Code Changes

See the [Pull Request Process](#pull-request-process) below.

## Development Setup

### Prerequisites

- Node.js ≥ 14
- npm or yarn
- MongoDB (local or cloud)
- Git
- Optional: Docker for containerized setup
- API keys for AI services (e.g., Google Cloud AI)

### Cloning the Repository

```bash
git clone https://github.com/decagondev/course-flow.git
cd course-flow
```

### Installing Dependencies

The project uses a monorepo structure with `/frontend` and `/backend`.

- Backend:
  ```bash
  cd backend
  npm install
  ```

- Frontend:
  ```bash
  cd ../frontend
  npm install
  ```

### Environment Configuration

Create `.env` files in both directories.

- Backend `.env` example:
  ```
  MONGO_URI=mongodb://localhost:27017/courseflow
  AI_API_KEY=your_ai_api_key
  PORT=3000
  JWT_SECRET=your_secret
  ```

- Frontend `.env` example:
  ```
  VITE_API_URL=http://localhost:3000/api
  VITE_AI_ENDPOINT=your_ai_endpoint
  ```

### Running the Project Locally

1. Start MongoDB (if local).
2. Backend:
   ```bash
   cd backend
   npm run dev
   ```
3. Frontend:
   ```bash
   cd frontend
   npm run dev
   ```
4. Access the app at `http://localhost:5173`.

For production build:
- Frontend: `npm run build`
- Backend: `npm run start`

## Branching Strategy

- **main**: Stable production branch.
- **develop**: Integration branch for features.
- Feature branches: `feature/<issue-number>-<short-description>`
- Bugfix branches: `bugfix/<issue-number>-<short-description>`
- Hotfix branches: `hotfix/<issue-number>-<short-description>`

Always branch from `develop` for new work.

## Commit Message Guidelines

Use Conventional Commits for consistency:

- Format: `<type>[optional scope]: <description>`
- Types: `feat` (new feature), `fix` (bug fix), `docs` (documentation), `style` (formatting), `refactor`, `test`, `chore`, `perf`, `ci`.
- Example: `feat(upload): add file validation for PDFs`

Keep messages concise (<50 chars for subject), and add a body if needed.

## Pull Request Process

1. Ensure your code addresses an open issue (or create one first).
2. Fork the repo and create your branch from `develop`.
3. Install dependencies and run tests: `npm test`.
4. Update documentation if your changes affect usage.
5. Commit your changes following the [guidelines](#commit-message-guidelines).
6. Push to your fork: `git push origin <branch-name>`.
7. Open a PR against `develop`.
   - Use the PR template.
   - Link to the issue (e.g., "Closes #123").
   - Describe changes, testing done, and screenshots for UI.
8. Request review from maintainers.
9. Address feedback and rebase if needed.
10. Once approved, a maintainer will merge.

PRs must pass CI checks (lint, tests) before merging.

## Testing Guidelines

- Write unit tests with Jest for backend and frontend logic.
- Integration tests for API endpoints (using supertest).
- E2E tests with Cypress for user flows.
- Aim for >80% coverage.
- Run `npm test` before committing.
- Add tests for new features/bug fixes.

## Style Guides

### JavaScript/TypeScript

- Use ESLint and Prettier (configured in the repo).
- Follow Airbnb style guide.
- Use TypeScript for type safety.
- Avoid `any`; prefer interfaces/types.

### CSS

- Use Tailwind CSS v4 utility classes.
- Prefer the CSS-first setup: add `@import "tailwindcss";` in your app stylesheet and, when needed, define design tokens with the `@theme` directive (no PostCSS or `tailwind.config.js` required by default).
- Keep custom CSS minimal; add tokens via `@theme` rather than sprawling bespoke CSS when possible.
- Ensure responsive design (mobile-first).

References: see Tailwind CSS v4 docs (`https://tailwindcss.com/blog/tailwindcss-v4`).

### Documentation

- Use Markdown.
- Keep it clear, concise, and up-to-date.
- Add JSDoc for functions/classes.

## Review Process

- PRs are reviewed within 48 hours (business days).
- Reviews check for: functionality, code quality, tests, docs.
- Use GitHub's review tools for comments.
- Approval requires at least one maintainer.

## Community and Support

- Discuss in [GitHub Discussions](https://github.com/decagondev/course-flow/discussions).
- Report security issues privately to [tom@decadev.co.uk](mailto:tom@decadev.co.uk).

## Acknowledgments

Thanks to all contributors! Your efforts make CourseFlow better.

Happy contributing! 🚀
