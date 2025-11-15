# Dev Skill Library

A comprehensive skill library for software development in Claude Code. This library provides expert-level skills for code review, testing, refactoring, documentation, deployment, and debugging.

## Features

This skill library includes specialized skills for:

- **Code Review**: Security review, performance analysis, and code quality assessment
- **Testing**: Unit test generation, integration testing, and coverage analysis
- **Refactoring**: Code refactoring, design patterns, and dependency management
- **Documentation**: API documentation and code documentation generation
- **Deployment**: CI/CD pipeline setup and Docker containerization
- **Debugging**: Bug analysis and error handling implementation

## Skills Overview

### Code Review

#### Security Review
Performs comprehensive security code review focusing on:
- Input validation (SQL injection, XSS, command injection)
- Authentication & authorization
- Data protection and encryption
- OWASP Top 10 vulnerabilities

Usage in Claude Code:
```
Use the security-review skill to analyze this codebase for security vulnerabilities
```

#### Performance Review
Analyzes code for performance issues:
- Algorithm efficiency (time/space complexity)
- Database query optimization
- Memory management
- Frontend performance
- Caching strategies

Usage:
```
Use the performance-review skill to identify performance bottlenecks
```

#### Code Quality Review
Ensures code maintainability and best practices:
- Code structure and organization
- Naming conventions
- Code duplication (DRY principle)
- Error handling
- Documentation quality

Usage:
```
Use the code-quality skill to review this code for maintainability
```

### Testing

#### Unit Test Generator
Generates comprehensive unit tests with:
- Complete test coverage (happy path, edge cases, errors)
- AAA pattern (Arrange, Act, Assert)
- Mocking external dependencies
- Parameterized tests

Usage:
```
Use the unit-test-generator skill to create tests for this module
```

#### Integration Test Generator
Creates integration tests for:
- Component interactions
- API endpoint testing
- Database operations
- End-to-end workflows

Usage:
```
Use the integration-test-generator skill to test these API endpoints
```

#### Test Coverage Analyzer
Analyzes test coverage and identifies gaps:
- Coverage metrics (line, branch, function)
- Missing test scenarios
- Risk assessment
- Prioritized recommendations

Usage:
```
Use the test-coverage-analyzer skill to evaluate our test suite
```

### Refactoring

#### Code Refactoring
Improves code quality through:
- Extract method/function
- Simplify conditionals
- Remove duplication
- Optimize performance
- Address code smells

Usage:
```
Use the code-refactor skill to refactor this module
```

#### Design Patterns
Identifies and implements design patterns:
- Creational patterns (Singleton, Factory, Builder)
- Structural patterns (Adapter, Decorator, Facade)
- Behavioral patterns (Strategy, Observer, Command)

Usage:
```
Use the design-patterns skill to improve the architecture
```

#### Dependency Cleanup
Analyzes and optimizes dependencies:
- Identify unused dependencies
- Find outdated packages
- Detect security vulnerabilities
- Suggest lighter alternatives

Usage:
```
Use the dependency-cleanup skill to optimize our dependencies
```

### Documentation

#### API Documentation Generator
Creates comprehensive API documentation:
- Endpoint documentation
- Request/response schemas
- Authentication details
- Code examples in multiple languages
- Error reference

Usage:
```
Use the api-docs-generator skill to document our REST API
```

#### Code Documentation Generator
Generates code documentation:
- Function/method documentation (JSDoc, docstrings, etc.)
- Class documentation
- Module documentation
- Type definitions
- Usage examples

Usage:
```
Use the code-docs-generator skill to document this codebase
```

### Deployment

#### CI/CD Pipeline Setup
Creates CI/CD pipelines for:
- GitHub Actions
- GitLab CI
- Jenkins
- Code quality checks, testing, building, and deployment

Usage:
```
Use the ci-cd-setup skill to create a GitHub Actions pipeline
```

#### Docker Setup
Containerizes applications with:
- Multi-stage Dockerfiles
- Docker Compose configurations
- Security best practices
- Image optimization
- Kubernetes deployment manifests

Usage:
```
Use the docker-setup skill to containerize this application
```

### Debugging

#### Bug Analyzer
Systematically analyzes and debugs issues:
- Root cause analysis
- Step-by-step debugging methodology
- Common bug patterns
- Fix recommendations
- Prevention strategies

Usage:
```
Use the bug-analyzer skill to debug this error
```

#### Error Handler
Implements robust error handling:
- Custom error classes
- Try-catch patterns
- Global error handlers
- Input validation
- Retry logic with backoff
- Structured logging

Usage:
```
Use the error-handler skill to implement error handling
```

## Installation

### Option 1: Clone and Link (Recommended for Development)

```bash
# Clone the repository
git clone https://github.com/ryanhe919/superskill.git

# Navigate to Claude Code skills directory
cd ~/.config/claude-code/skills  # Linux/Mac
# or
cd %APPDATA%/claude-code/skills  # Windows

# Create a symlink to the skills
ln -s /path/to/superskill/skills/* .
```

### Option 2: Copy Skills Directly

```bash
# Copy skills to Claude Code directory
cp -r superskill/skills/* ~/.config/claude-code/skills/
```

### Option 3: Use as Git Submodule

```bash
cd ~/.config/claude-code/skills
git submodule add https://github.com/ryanhe919/superskill.git
```

## Usage in Claude Code

Once installed, skills can be invoked in Claude Code using natural language:

```
Use the security-review skill to analyze this authentication module
```

```
Use the unit-test-generator skill to create tests for the UserService class
```

```
Use the docker-setup skill to containerize this Node.js application
```

## Skill Categories

```
skills/
├── code-review/
│   ├── security-review.md
│   ├── performance-review.md
│   └── code-quality.md
├── testing/
│   ├── unit-test-generator.md
│   ├── integration-test-generator.md
│   └── test-coverage-analyzer.md
├── refactoring/
│   ├── code-refactor.md
│   ├── design-patterns.md
│   └── dependency-cleanup.md
├── documentation/
│   ├── api-docs-generator.md
│   └── code-docs-generator.md
├── deployment/
│   ├── ci-cd-setup.md
│   └── docker-setup.md
└── debugging/
    ├── bug-analyzer.md
    └── error-handler.md
```

## Contributing

Contributions are welcome! To add a new skill:

1. Create a new markdown file in the appropriate category directory
2. Follow the existing skill format:
   - Clear description of what the skill does
   - Step-by-step methodology
   - Code examples
   - Best practices
   - Output format guidelines
3. Test the skill in Claude Code
4. Submit a pull request

## Skill Development Guidelines

Each skill should:
- Have a clear, focused purpose
- Provide systematic methodology
- Include practical examples
- Follow best practices
- Be language/framework agnostic when possible
- Provide actionable output

## License

MIT License

## Author

Created for use with Claude Code

## Support

For issues or questions:
- Open an issue on GitHub
- Check Claude Code documentation
- Review existing skills for examples

## Roadmap

Planned additions:
- Database optimization skills
- Architecture review skills
- Migration assistance skills
- Code generation templates
- Performance profiling skills
- Infrastructure as Code skills
