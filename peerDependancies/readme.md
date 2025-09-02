# NPM Peer Dependencies Guide

## Overview

This project demonstrates how to properly use `peerDependencies` in plugin and component library development. Learn how to prevent dependency duplication, ensure version compatibility, and create robust plugin architectures.

## What are Peer Dependencies?

Peer dependencies are packages that your library expects to be installed by the consuming application. Unlike regular dependencies, they are NOT automatically installed when your package is installed.

## Why Use Peer Dependencies?

1. **Prevent Duplication**: Avoid multiple instances of the same library
2. **Ensure Compatibility**: Guarantee compatible versions between host and plugin
3. **Reduce Bundle Size**: Share common libraries between packages
4. **Plugin Architecture**: Define clear host-plugin relationships

## Installation

For plugin consumers:
```bash
# Install the plugin
npm install my-react-components

# Install required peer dependencies
npm install react react-dom styled-components
```

## Peer Dependency Patterns

### React Component Library
```json
{
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    "react-dom": "^16.8.0 || ^17.0.0 || ^18.0.0"
  }
}
```

### Framework Plugin
```json
{
  "peerDependencies": {
    "webpack": "^5.0.0",
    "webpack-cli": "^4.0.0 || ^5.0.0"
  }
}
```

### UI Library with Theming
```json
{
  "peerDependencies": {
    "react": ">=16.8.0",
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

## SemVer in Peer Dependencies

### Caret Range (^)
- `^16.8.0` allows `>=16.8.0 <17.0.0`
- Compatible with minor and patch updates

### Tilde Range (~)
- `~16.8.0` allows `>=16.8.0 <16.9.0`
- Compatible with patch updates only

### OR Operator (||)
- `^16.8.0 || ^17.0.0 || ^18.0.0`
- Supports multiple major versions

### Exact Version
- `16.8.0` requires exactly this version
- Use sparingly, limits flexibility

## Handling Conflicts

### NPM v7+ Auto-Installation
```bash
# NPM v7+ automatically installs peer dependencies
npm install my-plugin
# Automatically installs react, react-dom, etc.
```

### Manual Conflict Resolution
```bash
# Check for conflicts
npm ls

# Install specific versions
npm install react@17.0.2 react-dom@17.0.2

# Force resolution (use carefully)
npm install --legacy-peer-deps
```

### Yarn Resolutions
```json
{
  "resolutions": {
    "react": "17.0.2",
    "react-dom": "17.0.2"
  }
}
```

## Development Best Practices

### 1. Version Range Strategy
```json
{
  "peerDependencies": {
    // Good: Flexible but bounded
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0",
    
    // Avoid: Too restrictive
    "lodash": "4.17.21",
    
    // Avoid: Too permissive
    "moment": "*"
  }
}
```

### 2. Optional Peer Dependencies
```json
{
  "peerDependencies": {
    "react": ">=16.8.0",
    "typescript": "^4.0.0"
  },
  "peerDependenciesMeta": {
    "typescript": {
      "optional": true
    }
  }
}
```

### 3. Runtime Checks
```javascript
// Check for peer dependency at runtime
function checkPeerDependency() {
  try {
    require('react');
  } catch (error) {
    throw new Error(
      'React is required as a peer dependency. Please install React.'
    );
  }
}
```

## Testing Strategies

### Test with Multiple Versions
```json
{
  "scripts": {
    "test:react16": "npm test",
    "test:react17": "npm test",
    "test:react18": "npm test",
    "test:all": "npm run test:react16 && npm run test:react17 && npm run test:react18"
  }
}
```

### CI Matrix Testing
```yaml
# .github/workflows/test.yml
strategy:
  matrix:
    react-version: ['16.14.0', '17.0.2', '18.2.0']
steps:
  - run: npm install react@${{ matrix.react-version }}
  - run: npm test
```

## Common Pitfalls

### 1. Missing Peer Dependencies
```bash
# Error: Cannot resolve dependency
Module not found: Error: Can't resolve 'react'
```

### 2. Version Conflicts
```bash
# Warning: Peer dependency version mismatch
ERESOLVE unable to resolve dependency tree
```

### 3. Bundle Duplication
```javascript
// Check for multiple React instances
if (typeof window !== 'undefined') {
  window.React1 = require('react');
  window.React2 = require('react');
  console.log(window.React1 === window.React2); // Should be true
}
```

## Migration Guide

### From Dependencies to Peer Dependencies
```json
{
  // Before
  "dependencies": {
    "react": "^17.0.0"
  },
  
  // After
  "peerDependencies": {
    "react": "^16.8.0 || ^17.0.0 || ^18.0.0"
  },
  "devDependencies": {
    "react": "^17.0.0"
  }
}
```

## Troubleshooting

### Check Installed Versions
```bash
npm ls react
npm ls react-dom
```

### Validate Peer Dependencies
```bash
npm install --dry-run
```

### Clear Cache
```bash
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

## Resources

- [NPM Peer Dependencies Documentation](https://docs.npmjs.com/cli/v8/configuring-npm/package-json#peerdependencies)
- [Semantic Versioning](https://semver.org/)
- [Yarn Resolutions](https://yarnpkg.com/configuration/yarnrc#selectiveVersionResolutions)