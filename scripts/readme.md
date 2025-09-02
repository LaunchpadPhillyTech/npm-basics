# NPM Scripts: Beyond "start" and "test"

## Overview

This project demonstrates advanced NPM scripting techniques that go beyond the basic `start` and `test` commands. Learn how to create powerful automation workflows, manage environments, and streamline your development process.

## Prerequisites

- Node.js >= 16.0.0
- npm >= 8.0.0
- Basic understanding of package.json

## Installation

```bash
npm install
```

## Available Scripts

### Development Scripts
- `npm run dev` - Start development server with hot reload and CSS watching
- `npm run start:dev` - Start server in development mode
- `npm run start:prod` - Start server in production mode

### Build Scripts
- `npm run build` - Build for production (automatically runs clean first)
- `npm run clean` - Remove build artifacts
- `npm run prebuild` - Pre-build setup (runs automatically)
- `npm run postbuild` - Post-build cleanup (runs automatically)

### Code Quality Scripts
- `npm run lint` - Run ESLint on all files
- `npm run format` - Format code with Prettier
- `npm run lint-and-format` - Run lint then format (only if lint passes)

### Database Scripts
- `npm run db:migrate` - Run database migrations
- `npm run db:seed` - Seed database with initial data
- `npm run db:setup` - Complete database setup (migrate + seed)

### Documentation Scripts
- `npm run docs` - Generate JSDoc documentation
- `npm run docs:serve` - Generate and serve documentation

### Testing Scripts
- `npm run test` - Run tests once
- `npm run test:watch` - Run tests in watch mode
- `npm run test:coverage` - Run tests with coverage report

## Script Execution Flow

### Build Process
```
npm run build
    ↓
1. prebuild (clean dist folder)
2. build (webpack compilation)
3. postbuild (optimization tasks)
```

### Development Workflow
```
npm run dev
    ↓
Concurrent execution:
- start:dev (nodemon server)
- watch:css (sass compilation)
```

## Environment Variables

Scripts support environment variable configuration:

```bash
# Development
NODE_ENV=development npm run start

# Production
NODE_ENV=production npm run start

# Custom build output
npm run build -- --output-path custom/dist
```

## Advanced Usage

### Chaining Scripts
```bash
# Run multiple scripts in sequence
npm run clean && npm run build && npm run test

# Run scripts in parallel
npm run dev & npm run docs:serve
```

### Passing Arguments
```bash
# Pass arguments to underlying tools
npm run lint -- --fix
npm run build -- --mode development
```

## Troubleshooting

### Common Issues

**Script not found:**
```bash
Error: missing script: script-name
```
Solution: Check script name in package.json scripts section

**Permission denied:**
```bash
EACCES: permission denied
```
Solution: Check file permissions or use `sudo` (not recommended for global installs)

**Environment variable not set:**
```bash
ReferenceError: NODE_ENV is not defined
```
Solution: Set environment variables before running scripts

## Best Practices

1. **Use lifecycle hooks** (pre/post) for setup and cleanup
2. **Chain related scripts** to ensure proper execution order
3. **Use meaningful script names** that describe their purpose
4. **Document script purposes** in README
5. **Handle errors gracefully** with proper exit codes
6. **Use environment variables** for configuration

## Contributing

1. Add new scripts to the appropriate section in package.json
2. Update this README with script documentation
3. Test scripts in both development and CI environments
4. Follow naming conventions (action:target pattern)