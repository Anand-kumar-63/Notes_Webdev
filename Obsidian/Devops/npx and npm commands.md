---
copilot-command-context-menu-enabled: true
copilot-command-slash-enabled: true
copilot-command-context-menu-order: 0
copilot-command-model-key: ""
copilot-command-last-used: 0
---
## npx serve
When you run `npx serve`:
- `npx` checks if the `serve` package is already available locally in your project's `node_modules` directory.
- If not found, `npx` temporarily downloads the `serve` package from the npm registry. 
- It then executes the `serve` command, which starts a static HTTP server, typically on port 5000 (or another available port).
- By default, `serve` will serve the files from the current directory where you executed the command. You can specify a different directory as an argument, for example, `npx serve public`.
- Once the server is stopped (e.g., by pressing Ctrl+C), `npx` cleans up the temporary installation of the `serve` package.
  In essence, `npx serve` provides a quick and convenient way to spin up a local static web server without needing to explicitly install the `serve` package globally or locally in your project.