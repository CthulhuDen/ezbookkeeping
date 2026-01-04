# ezBookkeeping Agent Guide

This documentation provides essential information for AI coding agents to work effectively within the `ezBookkeeping` repository.

## 1. Build, Lint, and Test Commands

### Backend (Go)

The backend is written in Go. Standard Go commands apply.

*   **Build:**
    ```bash
    CGO_ENABLED=1 go build -v -trimpath -ldflags "-w -s -linkmode external" -o ezbookkeeping ezbookkeeping.go
    ```

*   **Lint:**
    ```bash
    go vet ./...
    ```

*   **Test:**
    *   **Run All Tests:**
        ```bash
        go test ./...
        ```
    *   **Run Single Test:**
        ```bash
        go test -v -run TestName ./pkg/path/to/package
        ```
    *   **Skip Specific Tests:**
        ```bash
        # Use SKIP_TESTS environment variable
        SKIP_TESTS="TestName" go test ./...
        ```

### Frontend (Vue.js + TypeScript)

The frontend uses Vue 3, TypeScript, and Vite.

*   **Build:**
    ```bash
    npm run build
    ```

*   **Lint:**
    ```bash
    npm run lint
    # Runs: vue-tsc --noEmit && eslint . --fix
    ```

*   **Test:**
    *   **Run All Tests:**
        ```bash
        npm run test
        ```
    *   **Run Single Test:**
        ```bash
        # Use Jest's -t flag
        npm run test -- -t "Test Name"
        # Or directly with npx
        npx jest -t "Test Name"
        ```

## 2. Code Style & Conventions

### General
*   **Editor Config:** Respect `.editorconfig` settings (UTF-8, trim trailing whitespace, insert final newline).
*   **Naming:** Clear, descriptive names are preferred over abbreviations.

### Go (Backend)
*   **Formatting:** Use `gofmt` (tabs for indentation).
*   **Imports:** Group imports: Standard library first, then third-party libraries, then local `ezbookkeeping` packages.
*   **Naming:**
    *   `PascalCase` for exported types, functions, and fields.
    *   `camelCase` for private/internal ones.
    *   Package names should be short and lowercase (e.g., `pkg/core`, `pkg/api`).
*   **Error Handling:**
    *   Explicit error checking: `if err != nil { return err }`.
    *   Wrap errors with context when propagating if useful, or return custom errors from `pkg/errs`.
*   **Architecture:**
    *   `cmd/`: Entry points (webserver, etc.).
    *   `pkg/`: Library code.
    *   `pkg/api`: API handlers (Gin).
    *   `pkg/models`: Data models.
    *   `pkg/services`: Business logic.

### TypeScript / Vue (Frontend)
*   **Formatting:**
    *   Indentation: **4 Spaces** (as seen in `src/DesktopApp.vue` and `src/core/api.ts`). Note: `package.json` uses 2 spaces, but source code generally uses 4.
    *   Quotes: Single quotes `'` preferred.
    *   Semi-colons: Always use semi-colons `;`.
*   **Vue Components:**
    *   Use `<script setup lang="ts">`.
    *   PascalCase for component filenames (e.g., `DesktopApp.vue`).
*   **Naming:**
    *   `camelCase` for variables, functions, instances.
    *   `PascalCase` for classes, interfaces, types, components.
    *   `UPPER_CASE` for constants.
*   **Types:**
    *   Use explicit types (`interface`, `type`) where possible.
    *   Avoid `any` unless absolutely necessary.
    *   Interfaces like `ApiResponse<T>` in `src/core/api.ts` show generic usage patterns.

## 3. AI Agent Rules

*   **Context Awareness:** Always read related files (`read` tool) before modifying code to understand the local conventions and impact.
*   **Verification:** After writing code, ALWAYS run the relevant lint and test commands to ensure correctness.
*   **Security:** Do not hardcode secrets. Use the `settings` package or environment variables.
*   **Refactoring:** specific refactoring tasks should be confirmed with the user, but cleanup of unused imports/variables during related changes is encouraged.
