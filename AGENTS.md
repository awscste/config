# AGENTS.md - Agent Guidelines for This Project

## Project Overview

This is a minimal Go HTTP server project. The main entry point is a single Go file that runs an HTTP server on port 8080.

## Build, Lint, and Test Commands

### Running the Application

```bash
# Run the Go application
go run file

# Build the Go application
go build -o app file
```

### Linting

This project uses Checkstyle-IDEA with **Google Checks** (see `.idea/checkstyle-idea.xml`).

- For Java/Kotlin: Use Google Java Style Guide checks
- Run via IntelliJ IDEA or checkstyle command-line
- Suppress warnings with: `// CHECKSTYLE:OFF` and `// CHECKSTYLE:ON`

### Testing

No formal test framework is currently set up. To add tests:

```bash
# Run all tests
go test -v ./...

# Run a single test
go test -v -run TestFunctionName ./...

# Run tests with coverage
go test -v -cover ./...
```

## Code Style Guidelines

### General Principles

- Keep code simple and readable
- Follow Go idioms and conventions
- Use meaningful, descriptive names
- Write tests for all new functionality

### Naming Conventions

- **Variables/Functions**: `camelCase` (e.g., `greet`, `httpResponseWriter`)
- **Constants**: `PascalCase` or `camelCase` with prefix (e.g., `MaxConnections`)
- **Files**: `snake_case.go` (e.g., `main.go`, `http_handler.go`)
- **Packages**: short, lowercase, no underscores (e.g., `main`, `handlers`)

### Imports

- Group imports: standard library first, then third-party
- Use blank import `_` for side effects only
- Avoid unused imports (code will not compile)

```go
import (
    "fmt"
    "net/http"
    "time"

    "github.com/example/pkg"
)
```

### Formatting

- Use `gofmt` for automatic formatting
- Run `go fmt ./...` before committing
- Maximum line length: 100 characters (soft guideline)
- Use tabs for indentation, not spaces

### Types

- Use explicit types when it improves readability
- Prefer interface types for function parameters when appropriate
- Use pointers (`*Type`) for large structs to avoid copying
- Use value types for small, immutable data

### Error Handling

- Always check errors returned by functions
- Return meaningful error messages
- Use `fmt.Errorf` with `%w` for wrapped errors
- Handle errors at the appropriate level

```go
if err != nil {
    return fmt.Errorf("failed to process request: %w", err)
}
```

### HTTP Handlers

- Use `http.Handler` or `http.HandlerFunc` for HTTP endpoints
- Return appropriate HTTP status codes
- Log errors appropriately
- Validate input data

```go
func handler(w http.ResponseWriter, r *http.Request) {
    if r.Method != http.MethodGet {
        http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
        return
    }
    // handler logic
}
```

### Concurrency

- Use goroutines for concurrent operations
- Use channels for communication between goroutines
- Always handle race conditions with `sync.Mutex` or `sync.WaitGroup`
- Run `go test -race ./...` to detect race conditions

### Documentation

- Add doc comments for public functions and types
- Start with the name of the element being documented
- Keep comments concise but informative

```go
// Greet writes a greeting message to the response writer.
func Greet(w http.ResponseWriter, r *http.Request) {
    // ...
}
```

## Git Workflow

- Create feature branches from `main`
- Write meaningful commit messages
- Run `go fmt` and `go vet` before committing
- Ensure code compiles before pushing

## IDE Configuration

This project uses IntelliJ IDEA with the following configurations:

- **Checkstyle**: Google Checks (`.idea/checkstyle-idea.xml`)
- **JDK**: Version 20 (for Java/Kotlin files if added)
- **Language Level**: JDK 20

## Adding New Dependencies

1. Run `go get <package>` to add a dependency
2. Run `go mod tidy` to clean up go.mod and go.sum
3. Verify the change compiles correctly
