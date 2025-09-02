# Advanced Semantic Versioning (SemVer) Guide

## Overview

This project demonstrates advanced semantic versioning techniques for precise dependency management. Learn how to use version ranges effectively, handle prerelease versions, and create robust dependency strategies.

## Quick Reference

| Operator | Example | Range | Description |
|----------|---------|--------|-------------|
| `^` | `^1.2.3` | `>=1.2.3 <2.0.0` | Compatible changes |
| `~` | `~1.2.3` | `>=1.2.3 <1.3.0` | Patch updates only |
| `-` | `1.2.0 - 1.3.0` | `>=1.2.0 <=1.3.0` | Inclusive range |
| `*` | `1.2.*` | `>=1.2.0 <1.3.0` | Wildcard |
| `>=` | `>=1.2.0` | `>=1.2.0` | Greater than or equal |
| `<` | `<2.0.0` | `<2.0.0` | Less than |
| `\|\|` | `^1.0.0 \|\| ^2.0.0` | Multiple ranges | OR logic |

## Version Range Types

### Caret Ranges (^) - Compatible Changes
```json
{
  "dependencies": {
    "lodash": "^4.17.21",      // >=4.17.21 <5.0.0
    "react": "^18.2.0",        // >=18.2.0 <19.0.0
    "express": "^4.18.2"       // >=4.18.2 <5.0.0
  }
}
```

**Best for**: Libraries that follow SemVer strictly and maintain backward compatibility.

### Tilde Ranges (~) - Patch Updates Only
```json
{
  "dependencies": {
    "axios": "~1.2.3",         // >=1.2.3 <1.3.0
    "moment": "~2.29.4",       // >=2.29.4 <2.30.0
    "chalk": "~4.1.2"          // >=4.1.2 <4.2.0
  }
}
```

**Best for**: Critical dependencies where you want maximum stability.

### Hyphen Ranges - Inclusive Boundaries
```json
{
  "dependencies": {
    "webpack": "5.1.0 - 5.5.0",    // >=5.1.0 <=5.5.0
    "jest": "27.0.0 - 27.5.0",     // >=27.0.0 <=27.5.0
    "typescript": "4.5.0 - 4.9.0"  // >=4.5.0 <=4.9.0
  }
}
```

**Best for**: When you've tested specific version ranges.

### Wildcard Ranges - Flexible Matching
```json
{
  "dependencies": {
    "sass": "1.56.*",          // >=1.56.0 <1.57.0
    "postcss": "8.*",          // >=8.0.0 <9.0.0
    "eslint": "8.28.*"         // >=8.28.0 <8.29.0
  }
}
```

**Best for**: Simple, readable version constraints.

### Comparison Ranges - Precise Control
```json
{
  "dependencies": {
    "node-fetch": ">=2.6.7 <3.0.0",
    "sharp": ">0.32.0 <=0.33.0",
    "prisma": ">=4.7.0"
  }
}
```

**Best for**: Complex version requirements or excluding problematic versions.

### Combined Ranges - Multiple Compatibility
```json
{
  "dependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "node": ">=14.0.0 <16.0.0 || >=18.0.0",
    "webpack": "^4.0.0 || ^5.0.0"
  }
}
```

**Best for**: Supporting multiple major versions or skipping problematic ranges.

## Prerelease Versions

### Understanding Prereleases
```
1.0.0-alpha.1    # Alpha release
1.0.0-beta.2     # Beta release  
1.0.0-rc.1       # Release candidate
1.0.0            # Stable release
```

### Including Prereleases
```json
{
  "dependencies": {
    "next": "13.1.0-canary.1",
    "react": "^18.0.0-rc.0",
    "typescript": "^4.9.0-beta"
  }
}
```

### NPM Commands with Prereleases
```bash
# Install specific prerelease
npm install react@18.0.0-rc.0

# Install with prerelease tag
npm install next@canary

# Include prereleases in range resolution
npm install --include-prerelease
```

## Build Metadata

Build metadata is ignored for version precedence:

```
1.0.0+build.1
1.0.0+build.2
1.0.0+20231201
```

All considered equivalent for dependency resolution.

## NPM Configuration

### Save Prefix Configuration
```bash
# Use caret (default)
npm config set save-prefix='^'

# Use tilde
npm config set save-prefix='~'

# Use exact versions
npm config set save-prefix=''

# Check current setting
npm config get save-prefix
```

### Project-specific Configuration (.npmrc)
```
save-prefix=~
save-exact=true
package-lock=true
```

## Real-World Examples

### Frontend Application
```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.4.0",
    "axios": "~1.2.0",
    "lodash": "^4.17.21"
  },
  "devDependencies": {
    "webpack": "^5.75.0",
    "babel": "^7.20.0",
    "eslint": "~8.28.0",
    "jest": "^29.3.0",
    "typescript": "^4.9.0"
  }
}
```

### Node.js Backend
```json
{
  "dependencies": {
    "express": "^4.18.2",
    "mongoose": "^6.8.0",
    "jsonwebtoken": "~9.0.0",
    "bcrypt": "^5.1.0",
    "cors": "^2.8.5"
  },
  "devDependencies": {
    "nodemon": "^2.0.20",
    "supertest": "^6.3.3",
    "jest": "^29.3.0"
  }
}
```

### Library/Package
```json
{
  "dependencies": {
    "chalk": "~4.1.2",
    "yargs": "^17.6.0"
  },
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0"
  },
  "devDependencies": {
    "rollup": "^3.7.0",
    "@babel/core": "^7.20.0",
    "typescript": "^4.9.0"
  }
}
```

## Version Strategy Guidelines

### Stability Levels

**Maximum Stability** (Critical Production):
```json
{
  "dependencies": {
    "express": "4.18.2",        // Exact version
    "mongoose": "~6.8.0",       // Patch only
    "redis": "~4.5.0"          // Patch only
  }
}
```

**Balanced Approach** (Most Projects):
```json
{
  "dependencies": {
    "lodash": "^4.17.21",       // Minor + patch
    "axios": "^1.2.0",          // Minor + patch
    "moment": "~2.29.4"         // Patch only for legacy
  }
}
```

**Latest Features** (Development):
```json
{
  "dependencies": {
    "next": "^13.1.0",          // Latest features
    "react": "^18.2.0",         // Latest stable
    "tailwindcss": "^3.2.0"     // Latest utilities
  }
}
```

## Testing Strategy

### Multiple Version Testing
```json
{
  "scripts": {
    "test": "jest",
    "test:node14": "docker run --rm -v $(pwd):/app node:14 npm test",
    "test:node16": "docker run --rm -v $(pwd):/app node:16 npm test", 
    "test:node18": "docker run --rm -v $(pwd):/app node:18 npm test",
    "test:all": "npm run test:node14 && npm run test:node16 && npm run test:node18"
  }
}
```

### Dependency Auditing
```bash
# Check for vulnerabilities
npm audit

# Automated fixes
npm audit fix

# Check outdated packages
npm outdated

# Update within semver ranges
npm update
```

## Troubleshooting

### Common Issues

**Version Conflicts**:
```bash
# Check dependency tree
npm ls

# Find why package is installed
npm explain package-name

# Force resolution (use carefully)
npm install --force
```

**Lock File Issues**:
```bash
# Regenerate lock file
rm package-lock.json
npm install

# Verify integrity
npm ci
```

**Prerelease Problems**:
```bash
# Check available versions
npm view package-name versions --json

# Install specific version
npm install package-name@1.0.0-beta.1
```

## Best Practices

1. **Use lockfiles** (`package-lock.json`) for reproducible builds
2. **Test updates** before deploying to production
3. **Audit regularly** for security vulnerabilities
4. **Document version choices** in project README
5. **Automate updates** with tools like Renovate or Dependabot
6. **Monitor breaking changes** in major version updates
7. **Use exact versions** for critical dependencies
8. **Prefer stable releases** in production environments

## Tools and Resources

- **Renovate**: Automated dependency updates
- **Dependabot**: GitHub's dependency update tool
- **npm-check-updates**: Check for newer versions
- **semver.org**: Official semantic versioning specification
- **npm calculator**: Test version ranges online