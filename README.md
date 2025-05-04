# arcgis-experience-debug
Project with configs for debug in vscode

## Debugging in VSCode

### Debug Configuration

To facilitate the debugging process, the project includes specific configurations for VSCode that allow you to easily start and debug both the server and client.

### Starting Debugging

1. **Start server and client in debug mode**:
   - Press `Ctrl+Shift+P` to open the command palette
   - Type and select `Tasks: Run Task`
   - Select `start experience in debug`
   - This will start both the server and client in debug mode

2. **Select browser for debugging**:
   - After starting the server and client, press `Ctrl+Shift+D` to open the debug panel
   - In the debug configuration selector, choose the browser you want to use for debugging
   - Click the start debugging button (green play icon)
   - The selected browser will automatically open connected to the debugger


# Package.json Configuration Guide

This guide explains the package.json configurations for both the client and server components of Experience Builder.

## Server Configuration (server/package.json)

```json
{
  "scripts": {
    "start": "cross-env NODE_ENV=production node src/server",
    "install-windows-service": "winser -i --startcmd=\"node src/server\"",
    "uninstall-windows-service": "winser -r",
    "debug": "cross-env NODE_ENV=development nodemon src/server"
  }
}
```

### Server Scripts Explained

1. **start**
   ```bash
   cross-env NODE_ENV=production node src/server
   ```
   - Uses `cross-env` to set environment variables consistently across platforms
   - Sets `NODE_ENV` to "production" for optimized performance
   - Directly runs the server using Node.js
   - No auto-reload in production mode
   - Best for production deployment

2. **debug**
   ```bash
   cross-env NODE_ENV=development nodemon src/server
   ```
   - Sets `NODE_ENV` to "development"
   - Uses `nodemon` instead of `node`
   - Automatically restarts server when files change
   - Enables development features and detailed logging
   - Best for development and debugging

3. **Windows Service Scripts**
   ```bash
   # Installation
   winser -i --startcmd="node src/server"
   
   # Uninstallation
   winser -r
   ```
   - `install-windows-service`: Installs the server as a Windows service
   - `uninstall-windows-service`: Removes the Windows service
   - Uses `winser` package for Windows service management
   - Useful for production deployments on Windows servers

## Client Configuration (client/package.json)

```json
{
  "scripts": {
    "start": "cross-env NODE_ENV=development webpack --mode development --watch",
    "build:dev": "cross-env NODE_ENV=development webpack --mode development",
    "build:prod": "cross-env NODE_ENV=production OUTPUT_FOLDER=./dist-prod webpack --mode production",
    "test": "jest --runInBand",
    "lint": "eslint",
    "postinstall": "node ./npm-bootstrap-extensions"
  }
}
```

### Client Scripts Explained

1. **start**
   ```bash
   cross-env NODE_ENV=development webpack --mode development --watch
   ```
   - Sets development environment
   - Uses Webpack in development mode
   - Enables `--watch` flag for automatic rebuilds
   - Best for development workflow
   - Works with VSCode debugging configuration

2. **build:dev**
   ```bash
   cross-env NODE_ENV=development webpack --mode development
   ```
   - Creates a development build
   - No watch mode (single build)
   - Includes source maps
   - Minimal optimizations
   - Useful for testing development builds

3. **build:prod**
   ```bash
   cross-env NODE_ENV=production OUTPUT_FOLDER=./dist-prod webpack --mode production
   ```
   - Creates production-optimized build
   - Sets custom output folder (`dist-prod`)
   - Enables all production optimizations
   - Minifies and bundles code
   - Best for deployment

4. **test**
   ```bash
   jest --runInBand
   ```
   - Runs tests using Jest
   - `--runInBand` flag runs tests sequentially
   - Useful for CI/CD pipelines

5. **lint**
   ```bash
   eslint
   ```
   - Runs ESLint for code quality checks
   - Ensures code style consistency

6. **postinstall**
   ```bash
   node ./npm-bootstrap-extensions
   ```
   - Runs automatically after `npm install`
   - Sets up Experience Builder extensions
   - Initializes required configurations

## Key Dependencies and Their Purposes

### Common Dependencies
- `cross-env`: Ensures environment variables work across platforms
- `webpack`: Bundles and optimizes client-side code

### Server Dependencies
- `nodemon`: Development server with auto-reload
- `winser`: Windows service management

### Development Dependencies
- `jest`: Testing framework
- `eslint`: Code linting and style checking

## Environment Variables

### Server Environment Variables
- `NODE_ENV`
  - `development`: Enables debugging features
  - `production`: Optimizes for production use

### Client Environment Variables
- `NODE_ENV`
  - `development`: Includes source maps, minimal optimization
  - `production`: Full optimization, minification
- `OUTPUT_FOLDER`: Customizes build output location

## Best Practices

1. **Development Workflow**
   ```bash
   # Terminal 1 - Start server in debug mode
   cd server
   npm run debug

   # Terminal 2 - Start client in watch mode
   cd client
   npm start
   ```

2. **Production Build**
   ```bash
   # Build client
   cd client
   npm run build:prod

   # Start server
   cd server
   npm start
   ```

3. **Windows Service Deployment**
   ```bash
   cd server
   npm run install-windows-service
   ```

## Debugging Integration

The package.json scripts are integrated with VSCode's debug configuration:

1. **Client Debugging**
   - Uses the `start` script with development mode
   - Enables source maps for debugging
   - Integrates with Chrome DevTools

2. **Server Debugging**
   - Uses the `debug` script
   - Enables nodemon for auto-reload
   - Integrates with Node.js debugger

## Common Issues and Solutions

1. **Module Not Found Errors**
   - Run `npm install` in both client and server directories
   - Check if `postinstall` script completed successfully

2. **Windows Service Issues**
   - Ensure you have administrative privileges
   - Check Windows Event Viewer for errors
   - Verify server path in service configuration

3. **Build Performance**
   - Use `start` script during development
   - Only use `build:prod` for production releases
   - Consider using `build:dev` for faster builds during testing




![image](https://github.com/user-attachments/assets/d658a74a-ae42-4071-b080-d2ef72cf3435)

