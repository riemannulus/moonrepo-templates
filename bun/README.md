# Bun Project Template

## Description

Initialize a new Bun project with TypeScript support. Choose between a minimal setup, basic project, or React project with optional TailwindCSS and shadcn/ui components.

## Usage

```bash
moon generate bun ./my-bun-project
```

## Variables

- **name**: Project name (required) - Used for the project title and directory name
- **description**: Project description (optional) - Used in package.json and documentation
- **author**: Author name (optional) - Added to package.json
- **project_type**: Select project type (required) - Options:
  - `minimal`: Basic TypeScript setup with minimal dependencies
  - `basic`: Full Bun project with HTTP server
  - `react`: React application with basic setup
  - `react-tailwind`: React application with TailwindCSS
  - `react-shadcn`: React application with TailwindCSS and shadcn/ui components
- **include_tests**: Include test setup (optional, default: true)

## Generated Files

### Core Files (All Projects)
- `package.json` - Dependencies and scripts
- `tsconfig.json` - TypeScript configuration
- `bunfig.toml` - Bun configuration
- `README.md` - Project documentation

### Source Files
- **Minimal**: `src/index.ts` - Simple TypeScript module
- **Basic**: `src/index.ts` - HTTP server with Bun
- **React**: `src/index.tsx`, `src/App.tsx` - React application

### Conditional Files
- **TailwindCSS**: `tailwind.config.js`, `src/styles.css`
- **shadcn/ui**: `src/components/ui/` - UI components and utilities
- **Tests**: `src/*.test.ts(x)`, `src/setup-tests.ts`
- **React**: `index.html`

## Project Types

### Minimal
- Basic TypeScript setup
- Minimal dependencies
- Simple function export

### Basic  
- Full Bun project with HTTP server
- Health check endpoint
- Development and production scripts

### React
- React 18 with TypeScript
- Modern React patterns (hooks, functional components)
- Counter example application

### React + TailwindCSS
- All React features
- TailwindCSS for styling
- Responsive design utilities

### React + shadcn/ui
- All TailwindCSS features
- shadcn/ui component library
- Pre-configured design system
- Button and Card components included

## Features

- **Fast**: Leverages Bun's speed for development and builds
- **Modern**: Uses latest TypeScript and React features
- **Flexible**: Multiple project configurations
- **Testing**: Optional test setup with Bun test runner
- **Styling**: Optional TailwindCSS and shadcn/ui integration
- **Development**: Watch mode and hot reloading support 