# Agent Guidelines for VOSviewer Online

This document provides guidelines for agentic coding systems (AI agents) working in this repository.

## Build, Lint, and Test Commands

### Building
```bash
# Build main VOSviewer app
npm run build:vosviewer

# Build all variants
npm run build:dimensions        # Build Dimensions variant
npm run build:zeta-alpha        # Build Zeta Alpha variant
npm run build:rori              # Build RoRI variant
npm run build-component-package # Build npm component package (all variants)

# Development server
npm run dev:vosviewer           # Start dev server on port 8600
npm run dev:dimensions          # Start dev server for Dimensions variant
npm run dev:zeta-alpha          # Start dev server for Zeta Alpha variant
npm run dev:rori                # Start dev server for RoRI variant
```

### Linting
```bash
# ESLint is configured but no lint command is defined in package.json
# Run ESLint directly if needed:
npx eslint src/
npx eslint src/ --fix           # Auto-fix linting issues
```

### Testing
**Note:** This project does not have automated tests configured. Manual testing is required.

## Code Style Guidelines

### Imports
- Use absolute imports from `/src/` (configured in `.eslintrc`)
- Group imports: React/third-party libraries, then project imports
- Example:
  ```javascript
  import React, { useRef } from 'react';
  import _get from 'lodash/get';
  import ConfigStore from 'store/config';
  ```
- Prefer lodash utilities (prefixed with `_`) for data operations
- Use named imports for specific utilities, default imports for modules

### Formatting
- **Indentation:** 2 spaces (enforced by ESLint)
- **Line breaks:** Ignored (linebreak-style: 0)
- **Quotes:** No preference enforced (quotes: 0)
- **Max line length:** No limit enforced (max-len: 0)
- **Comma dangle:** Allowed (comma-dangle: 0)
- **Object formatting:** Multi-line objects require at least 6 properties

### Types
- **No TypeScript.** This is a Babel-based JavaScript project using JSX
- `.babelrc` includes decorators, class properties, and object spread
- Use PropTypes sparingly (react/prop-types: 0)

### Naming Conventions
- **Components:** PascalCase (e.g., `VOSviewerApp`, `ErrorDialog`)
- **Functions/variables:** camelCase (e.g., `getStore`, `parseResults`)
- **Private/helper functions:** Prefix with underscore (e.g., `_checkNoData`)
- **React Context:** PascalCase with `Context` suffix (e.g., `ConfigStoreContext`)
- **Providers:** PascalCase with `Provider` suffix (e.g., `ConfigProvider`)
- **Constants:** UPPER_SNAKE_CASE in variables.js

### Project Structure
```
src/
  ├── components/          # Reusable UI components
  ├── pages/               # Page-level components
  ├── store/               # MobX state management (stores + providers)
  ├── utils/               # Utility functions and helpers
  ├── workers/             # Web workers
  ├── VOSviewerApp.js      # Main app component
  └── index.js             # Entry point
```

### State Management
- Uses **MobX** with React Context API
- Store pattern: One store per domain (config, data, ui, etc.)
- Lazy initialization using `useRef` to avoid recreating stores
- Each store has a corresponding Provider component
- Example pattern in `store/stores.js`

### Error Handling
- Errors are objects with `key`, `message`, `dataType`, and optional `lineNumber`
- Use `errorKeys` and `errorMessages` from `utils/variables.js`
- Export error validation functions like `getMapDataError()`, `getNetworkDataError()`
- Helper functions for validation are private (prefixed with `_`)
- Return `null` or `undefined` when no error is found
- Check for `undefined`, `null`, and empty strings explicitly
- Example: `if (_isUndefined(id) || _isNull(id) || id === '')`

### ESLint Configuration
Key rules enforced (`.eslintrc`):
- `no-undef`: error (prevent undefined variables)
- `no-const-assign`: error (prevent reassigning constants)
- `prefer-const`: error (use const by default)
- `indent`: 2 spaces (SwitchCase: 1)
- `react/jsx-indent`: 2 spaces
- `react/jsx-pascal-case`: PascalCase component names
- `no-param-reassign`: error (props cannot be mutated, but object props can)
- `no-plusplus`: error except in for loops
- `dependencies/no-cycles`: error (prevent circular dependencies)
- `emotion/syntax-preference`: object notation (not styled-components)

Rules disabled:
- `jsx-a11y/mouse-events-have-key-events`
- `jsx-a11y/click-events-have-key-events`
- `no-shadow`
- `max-len`
- `no-console`

### Emotion Styling
- Use `@emotion/react` and `@emotion/styled` (not Styled Components)
- Emotion syntax preference: object notation (not CSS strings)
- Import: `import { css } from '@emotion/react'`

### Dependencies
- Material-UI v5 (`@mui/material`, `@mui/icons-material`)
- MobX v6 for state management
- D3 v7 for visualization
- React v17
- Lodash for utility functions
- No tests (Jest not configured)

### File Naming
- JavaScript files: `.js` extension (no `.jsx`)
- Components can have `.js` extension
- Directories for component groups: PascalCase (e.g., `/ErrorDialog`)

## Development Workflow

1. Make changes and test locally with `npm run dev:vosviewer`
2. Run ESLint to check for style issues: `npx eslint src/ --fix`
3. Build for production: `npm run build:vosviewer`
4. Test all build variants before committing
5. Builds output to `dist/` directory

## Key Files to Know

- `.eslintrc` - ESLint configuration
- `.babelrc` - Babel configuration
- `package.json` - Dependencies and build scripts
- `src/store/` - MobX stores and context providers
- `src/utils/errors.js` - Error handling utilities
- `webpack.config.babel.js` - Main webpack configuration
- `webpack.component.config.babel.js` - Component package webpack configuration
