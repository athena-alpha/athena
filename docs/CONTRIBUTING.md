# Contributing

As a Free and Open Source Software (FOSS) project, we welcome and greatly appreciate any new code contributions!

## Table Of Contents
- [General Practices](#general-practices)
- [Class, Function & Variable Formats](#class-function--variable-formats)
- [Small Code Additions (<100 Lines)](#small-code-additions-100-lines)
- [Larger Code Additions (>100 Lines)](#larger-code-additions-100-lines)

## General Practices
We strive to build all code to industry standard best practices, whilst minimizing third party dependencies. We write modular, concise, easily maintainable and clearly commented code that adheres to the latest, industry-leading privacy and security standards, drawing guidance from organizations including the Open Web Application Security Project (OWASP) and the Electronic Frontier Foundation (EFF). Follow these guidelines to ensure smooth collaboration.

- Use existing code, such as components/hooks for the Client or Models/Repositories/Services/Controllers for the Server, as much as possible
- Follow the general style of the existing code base, don't try and invent your own
- Do not place `common` code outside the common directory, don't place `<domain>` code outside its respective domain directory
- Do not mix `common` or `<domain>` code in with each other
- Run linting such as `npx eslint .` for the Client and `composer run lint` for the Server
- Type check code for the Client using `npx tsc --noEmit`
- Do not commit secrets (API keys, secret files etc)

## Class, Function & Variable Formats
- **Client:** ECMAScript Standards: Classes use `PascalCase` | Functions use `camelCase` | Variables use `camelCase`
  - **Enums:** Use `PascalCase`
  - **Global Constants:** Use `camelCase`
  - **i18n Translations:** Use `camelCase`
- **Server:** PSR-12 Standards: Classes use `PascalCase` | Functions use `camelCase` | Variables use `camelCase`
- **Database:** Tables use `snake_case` | Columns use `snake_case`
- **Returned API Data:** CMAScript Standards: Classes use `PascalCase` | Functions use `camelCase` | Variables use `camelCase`

## Small Code Additions (<100 Lines)
For small code commits, you're welcome to just open a pull request with your new code as per the process below. Do please:
- Fully comment your new code using JSDoc for TypeScript and PHPDoc for PHP (or get AI to do it) so it's clear what its functionality is
- Write clear, concise commit messages summarizing the change
- Avoid submitting any commented-out code
- Ensure the code fully builds before submitting
```bash
docker compose up --build
```

## Larger Code Additions (>100 Lines)
For large changes, please open a GitHub issue to discuss first before coding. Please also ensure significant new code adheres to the following standards:

### Client (TypeScript)
- Follow [ESLint rules](https://github.com/athena-alpha/athena-client/blob/main/eslint.config.js)
- Components should be located in the `components` folder inside the domain they're for
- Constants should be located in the `constants` folder inside the domain they're for
- Pages should be located in the `pages` folder inside the domain they're for
- Add TypeScript types for props and state
  - For types that are not reused, they can stay in the same file
  - For types that are reused please place them in the dedicated `types` folder inside the domain they're for
- Utils should be located in the `utils` folder inside the domain they're for

### Server (PHP)
- Adhere to PSR-12 standards
- Use dependency injection in Slim controllers

### Process
1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Commit changes: `git commit -m "Add my feature"`
4. Push: `git push origin feature/my-feature`
5. Open a pull request with a clear description, including the problem solved and testing done
6. For large changes, discuss via a GitHub issue first

PRs are reviewed when we have time to do so. Expect feedback on code quality, security, and standards.
