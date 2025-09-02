# NPM Basics: Complete Tutorial

A comprehensive guide to mastering NPM (Node Package Manager) fundamentals, covering essential concepts for modern JavaScript development.

## 📋 Table of Contents

1. [Overview](#overview)
2. [Prerequisites](#prerequisites)
3. [NPM Scripts: Beyond Basics](#npm-scripts-beyond-basics)
4. [Semantic Versioning (SemVer)](#semantic-versioning-semver)
5. [NPM Engines Field](#npm-engines-field)
6. [Peer Dependencies](#peer-dependencies)
7. [Best Practices](#best-practices)
8. [Common Workflows](#common-workflows)
9. [Troubleshooting](#troubleshooting)

## Overview

This tutorial covers four critical NPM concepts that every JavaScript developer should master:

- **NPM Scripts**: Create powerful automation workflows beyond simple `start` and `test` commands
- **Semantic Versioning**: Master version ranges and dependency management strategies
- **Engines Field**: Control and specify required Node.js and NPM versions for consistent environments
- **Peer Dependencies**: Build plugins and component libraries with proper dependency management

## Prerequisites

- Node.js >= 16.0.0
- npm >= 8.0.0
- Basic understanding of JavaScript and package.json
- Familiarity with command line interface

## NPM Scripts: Beyond Basics

### Understanding NPM Scripts

NPM scripts are shell commands defined in your `package.json` that automate common development tasks. They provide a consistent interface across different operating systems and development environments.

### Advanced Script Examples

```json
{
  "scripts": {
    "start": "node src/index.js",
    "start:dev": "NODE_ENV=development nodemon src/index.js",
    "start:prod": "NODE_ENV=production node src/index.js",
    "dev": "concurrently \"npm run start:dev\" \"npm run watch:css\"",
    
    "build": "webpack --mode production",
    "prebuild": "npm run clean && echo 'Starting build process...'",
    "postbuild": "npm run optimize && echo 'Build completed!'",
    "clean": "rimraf dist coverage docs",
    
    "lint": "eslint src --ext .js,.jsx,.ts,.tsx",
    "lint:fix": "eslint src --ext .js,.jsx,.ts,.tsx --fix",
    "format": "prettier --write src/**/*.{js,jsx,ts,tsx,json,css,md}",
    "lint-and-format": "npm run lint && npm run format",
    
    "test": "jest",
    "test:watch": "jest --watchAll",
    "test:coverage": "jest --coverage",
    "test:ci": "jest --ci --coverage --watchAll=false",
    
    "db:migrate": "sequelize db:migrate",
    "db:seed": "sequelize db:seed:all",
    "db:setup": "npm run db:migrate && npm run db:seed",
    
    "docs": "jsdoc -c jsdoc.conf.json -d docs src",
    "docs:serve": "npm run docs && serve docs"
  }
}
```

### Script Execution Patterns

#### Pre/Post Hooks
NPM automatically runs `pre` and `post` scripts:
```bash
npm run build
# Automatically runs: prebuild → build → postbuild
```

#### Environment Variables
```bash
NODE_ENV=production npm run start
npm run build -- --output-path custom/dist
```

#### Sequential vs Parallel Execution
```bash
# Sequential (&&)
npm run clean && npm run build && npm run test

# Parallel (&)
npm run dev & npm run docs:serve

# Using concurrently package
npm run dev  # Runs multiple processes concurrently
```

## Semantic Versioning (SemVer)

### Version Format: MAJOR.MINOR.PATCH

- **MAJOR**: Incompatible API changes
- **MINOR**: Backward-compatible functionality additions
- **PATCH**: Backward-compatible bug fixes

### Version Range Operators

| Operator | Example | Range | Description |
|----------|---------|--------|-------------|
| `^` | `^1.2.3` | `>=1.2.3 <2.0.0` | Compatible changes |
| `~` | `~1.2.3` | `>=1.2.3 <1.3.0` | Patch updates only |
| `-` | `1.2.0 - 1.3.0` | `>=1.2.0 <=1.3.0` | Inclusive range |
| `*` | `1.2.*` | `>=1.2.0 <1.3.0` | Wildcard |
| `>=` | `>=1.2.0` | `>=1.2.0` | Greater than or equal |
| `<` | `<2.0.0` | `<2.0.0` | Less than |
| `\|\|` | `^1.0.0 \|\| ^2.0.0` | Multiple ranges | OR logic |

### Practical Examples

#### Caret Ranges (^) - Compatible Changes
```json
{
  "dependencies": {
    "lodash": "^4.17.21",      // >=4.17.21 <5.0.0
    "react": "^18.2.0",        // >=18.2.0 <19.0.0
    "express": "^4.18.2"       // >=4.18.2 <5.0.0
  }
}
```
**Use case**: Libraries that follow SemVer strictly

#### Tilde Ranges (~) - Patch Updates Only
```json
{
  "dependencies": {
    "axios": "~1.2.3",         // >=1.2.3 <1.3.0
    "moment": "~2.29.4",       // >=2.29.4 <2.30.0
    "chalk": "~4.1.2"          // >=4.1.2 <4.2.0
  }
}
```
**Use case**: Critical dependencies requiring maximum stability

#### Combined Ranges - Multiple Compatibility
```json
{
  "dependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "node": ">=14.0.0 <16.0.0 || >=18.0.0",
    "webpack": "^4.0.0 || ^5.0.0"
  }
}
```
**Use case**: Supporting multiple major versions

### Prerelease Versions
```json
{
  "dependencies": {
    "typescript": "^4.9.0-beta",     // Beta releases
    "react": "^18.3.0-alpha.1",      // Alpha releases  
    "webpack": "^5.75.0-rc.1"        // Release candidates
  }
}
```

## NPM Engines Field

### Purpose and Benefits

The `engines` field specifies which versions of Node.js, NPM, and other tools your project requires:

1. **Prevent Installation Errors**: Stop incompatible environments before problems occur
2. **Ensure Consistent Behavior**: Guarantee same features across all environments
3. **Improve Team Collaboration**: Align everyone on the same toolchain
4. **Enforce CI/CD Standards**: Maintain consistent deployment pipelines

### Basic Syntax

```json
{
  "engines": {
    "node": ">=16.0.0",
    "npm": ">=8.0.0"
  }
}
```

### Advanced Engine Configurations

#### Production Application (Strict)
```json
{
  "engines": {
    "node": "16.14.0",
    "npm": "8.5.0"
  }
}
```

#### Library (Flexible)
```json
{
  "engines": {
    "node": ">=16.0.0 <20.0.0",
    "npm": ">=8.5.0 <11.0.0",
    "yarn": ">=1.22.0 <4.0.0",
    "pnpm": ">=7.0.0 <9.0.0"
  }
}
```

#### Multi-Platform Support
```json
{
  "engines": {
    "node": "14.x || >=16.0.0",
    "npm": "6.x || >=8.0.0"
  }
}
```

### Package Manager Specification
```json
{
  "packageManager": "pnpm@8.6.0",
  "engines": {
    "pnpm": ">=8.0.0"
  }
}
```

### Environment Checking Scripts
```json
{
  "scripts": {
    "preinstall": "node scripts/check-environment.js",
    "postinstall": "node scripts/verify-installation.js",
    "check-env": "node scripts/check-environment.js"
  }
}
```

## Peer Dependencies

### What are Peer Dependencies?

Peer dependencies are packages that your library expects the consuming application to provide. They prevent duplication and ensure compatibility between host applications and plugins.

### When to Use Peer Dependencies

- **React/Vue Component Libraries**: Require React/Vue to be installed by the consumer
- **Framework Plugins**: Expect the framework to be present
- **Tooling Extensions**: Build on existing tools like Webpack, Babel, etc.

### Basic Example

```json
{
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "react-dom": "^16.8.0 || ^17.0.0 || ^18.0.0"
  }
}
```

### Advanced Peer Dependencies with Optional Support

```json
{
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "react-dom": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "styled-components": "^5.0.0 || ^6.0.0",
    "@emotion/react": "^11.0.0",
    "@emotion/styled": "^11.0.0"
  },
  "peerDependenciesMeta": {
    "styled-components": {
      "optional": true
    },
    "@emotion/react": {
      "optional": true
    },
    "@emotion/styled": {
      "optional": true
    }
  }
}
```

### Installation Process

For consumers of libraries with peer dependencies:

```bash
# Install the library
npm install @mycompany/react-ui-components

# Install required peer dependencies (NPM 7+ does this automatically)
npm install react react-dom styled-components

# Check for peer dependency warnings
npm ls
```

### Common Patterns

#### Framework Plugin
```json
{
  "peerDependencies": {
    "webpack": "^5.0.0",
    "webpack-cli": "^4.0.0 || ^5.0.0"
  }
}
```

#### Testing Framework Extension
```json
{
  "peerDependencies": {
    "jest": "^27.0.0 || ^28.0.0 || ^29.0.0",
    "@testing-library/react": "^12.0.0 || ^13.0.0"
  }
}
```

## Best Practices

### 1. Version Management
- Use `^` for most dependencies (allows compatible updates)
- Use `~` for critical dependencies (patch updates only)
- Pin exact versions for deployment tools
- Regularly audit and update dependencies

### 2. Script Organization
- Use descriptive script names
- Group related scripts with prefixes (`test:`, `build:`, `db:`)
- Leverage pre/post hooks for automation
- Document complex scripts in README

### 3. Environment Control
- Always specify `engines` field for projects
- Use `packageManager` field for monorepos
- Include environment checking scripts
- Test across multiple Node.js versions

### 4. Peer Dependencies
- Be generous with version ranges
- Use `peerDependenciesMeta` for optional dependencies
- Provide clear installation instructions
- Test with minimum supported versions

## Common Workflows

### Development Workflow
```bash
# Initial setup
npm install
npm run check-env

# Development
npm run dev          # Start dev server + file watching
npm run lint:fix     # Fix linting issues
npm run test:watch   # Run tests in watch mode

# Before committing
npm run lint-and-format
npm run test
npm run build
```

### Release Workflow
```bash
# Version bumping
npm run test           # Ensure all tests pass
npm run build         # Build for production
npm version patch     # Bump version (patch/minor/major)
npm publish          # Publish to registry

# Or automated release
npm run release       # Combined test + build + version + publish
```

### Dependency Management
```bash
# Check for updates
npm outdated
npm audit

# Update dependencies
npm update            # Update within semver ranges
npm audit fix        # Fix security vulnerabilities

# Major updates (requires npm-check-updates)
ncu -u               # Update package.json
npm install          # Install new versions
```

## Troubleshooting

### Common Issues

#### Peer Dependency Conflicts
```bash
npm ls                # Check dependency tree
npm install --legacy-peer-deps  # Use legacy resolution
```

#### Engine Compatibility
```bash
node --version        # Check current Node.js version
npm --version         # Check current NPM version
npm config get registry  # Verify registry
```

#### Script Execution Problems
```bash
npm config list       # Check NPM configuration
npm run env           # See all environment variables
npm run <script> --silent  # Reduce verbose output
```

### Debug Commands

```bash
npm config list -l    # List all configuration
npm cache clean --force  # Clear NPM cache
npm doctor            # Run NPM diagnostic
npm fund              # Check funding information
npm audit --audit-level high  # Check for high-severity issues
```

## Conclusion

Mastering these NPM concepts will significantly improve your JavaScript development workflow:

- **NPM Scripts** automate repetitive tasks and create consistent development experiences
- **Semantic Versioning** ensures predictable dependency management and reduces conflicts
- **Engines Field** guarantees consistent environments across development, testing, and production
- **Peer Dependencies** enable flexible plugin architectures and prevent dependency duplication

Continue practicing these concepts in real projects to build expertise and develop your own workflow preferences.

---

## Additional Resources

- [NPM Official Documentation](https://docs.npmjs.com/)
- [Semantic Versioning Specification](https://semver.org/)
- [Node.js Version Management](https://nodejs.org/en/download/releases/)
- [Package.json Documentation](https://docs.npmjs.com/cli/v9/configuring-npm/package-json)
