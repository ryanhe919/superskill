# Usage Examples

This document provides practical examples of how to use each skill in the Dev Skill Library with Claude Code.

## Code Review Skills

### Security Review

**Scenario**: You want to check if your authentication module has security vulnerabilities.

**Usage**:
```
Use the security-review skill to analyze the authentication module in src/auth/
```

**Expected Output**:
- List of security vulnerabilities categorized by severity
- Specific file locations and line numbers
- Risk assessment for each finding
- Concrete recommendations with code examples
- References to security best practices

### Performance Review

**Scenario**: Your API endpoints are slow and you need to identify bottlenecks.

**Usage**:
```
Use the performance-review skill to analyze the API handlers in src/api/handlers/
```

**Expected Output**:
- Performance issues categorized (Algorithm, Database, Memory, etc.)
- Time complexity analysis
- Specific optimization recommendations
- Before/after code comparisons
- Expected performance improvements

### Code Quality Review

**Scenario**: You want to improve the maintainability of a legacy module.

**Usage**:
```
Use the code-quality skill to review src/legacy/order-processor.js
```

**Expected Output**:
- Code structure issues
- Naming convention problems
- Code duplication instances
- Refactoring recommendations
- Improved code examples

## Testing Skills

### Unit Test Generator

**Scenario**: You've written a new service class and need comprehensive tests.

**Usage**:
```
Use the unit-test-generator skill to create tests for src/services/UserService.ts
```

**Expected Output**:
- Complete test suite with describe/it blocks
- Tests for all public methods
- Edge cases and error scenarios
- Mocked dependencies
- Setup and teardown code

**Example Output**:
```typescript
describe('UserService', () => {
  let userService: UserService;
  let mockUserRepository: jest.Mocked<UserRepository>;

  beforeEach(() => {
    mockUserRepository = {
      findById: jest.fn(),
      save: jest.fn(),
    };
    userService = new UserService(mockUserRepository);
  });

  describe('getUser', () => {
    it('should return user when user exists', async () => {
      const mockUser = { id: 1, name: 'John' };
      mockUserRepository.findById.mockResolvedValue(mockUser);

      const result = await userService.getUser(1);

      expect(result).toEqual(mockUser);
      expect(mockUserRepository.findById).toHaveBeenCalledWith(1);
    });

    it('should throw NotFoundError when user does not exist', async () => {
      mockUserRepository.findById.mockResolvedValue(null);

      await expect(userService.getUser(1)).rejects.toThrow(NotFoundError);
    });
  });
});
```

### Integration Test Generator

**Scenario**: You need integration tests for your REST API endpoints.

**Usage**:
```
Use the integration-test-generator skill to test the /api/users endpoints
```

**Expected Output**:
- Complete integration test suite
- Database setup and teardown
- Real HTTP requests
- Multi-step workflows
- Data fixture management

### Test Coverage Analyzer

**Scenario**: You want to know which parts of your code lack test coverage.

**Usage**:
```
Use the test-coverage-analyzer skill to evaluate test coverage for src/
```

**Expected Output**:
- Coverage summary with percentages
- Untested files and functions
- Missing test scenarios
- Prioritized recommendations
- Risk assessment

## Refactoring Skills

### Code Refactoring

**Scenario**: You have a large function that's hard to maintain.

**Usage**:
```
Use the code-refactor skill to refactor the processOrder function in src/order.js
```

**Expected Output**:
- Identified code smells
- Refactoring patterns to apply
- Before/after code comparison
- Step-by-step refactoring plan
- Impact assessment

**Example**:

**Before**:
```javascript
function processOrder(order) {
  // 150 lines of nested if statements and multiple responsibilities
  if (order.items.length > 0) {
    let total = 0;
    for (let item of order.items) {
      if (item.discount) {
        total += item.price * (1 - item.discount);
      } else {
        total += item.price;
      }
    }
    if (order.customer.isPremium) {
      total *= 0.9;
    }
    // ... 100+ more lines
  }
}
```

**After**:
```javascript
function processOrder(order) {
  const total = calculateTotal(order);
  const discount = applyCustomerDiscount(total, order.customer);
  return createOrderSummary(order, discount);
}

function calculateTotal(order) {
  return order.items.reduce((sum, item) =>
    sum + calculateItemPrice(item), 0
  );
}

function calculateItemPrice(item) {
  return item.discount
    ? item.price * (1 - item.discount)
    : item.price;
}

function applyCustomerDiscount(total, customer) {
  return customer.isPremium ? total * 0.9 : total;
}
```

### Design Patterns

**Scenario**: Your code has tight coupling between components.

**Usage**:
```
Use the design-patterns skill to improve the notification system architecture
```

**Expected Output**:
- Applicable design patterns identified
- Pattern implementation with code
- Usage examples
- Trade-offs and benefits
- Testing approach

### Dependency Cleanup

**Scenario**: Your project has accumulated many dependencies and you want to optimize.

**Usage**:
```
Use the dependency-cleanup skill to analyze package.json
```

**Expected Output**:
- List of unused dependencies
- Outdated packages with update recommendations
- Security vulnerabilities (CVEs)
- Bundle size optimizations
- Action plan with commands

## Documentation Skills

### API Documentation Generator

**Scenario**: You need to document your REST API for external developers.

**Usage**:
```
Use the api-docs-generator skill to document the REST API in src/api/
```

**Expected Output**:
- Complete API reference documentation
- Endpoint descriptions with HTTP methods
- Request/response schemas
- Authentication details
- Code examples in multiple languages
- Error response documentation

### Code Documentation Generator

**Scenario**: You need to add JSDoc comments to your JavaScript codebase.

**Usage**:
```
Use the code-docs-generator skill to document src/utils/
```

**Expected Output**:
- Comprehensive JSDoc comments
- Type definitions
- Parameter descriptions
- Return value documentation
- Usage examples
- Links to related functions

## Deployment Skills

### CI/CD Pipeline Setup

**Scenario**: You want to set up automated testing and deployment with GitHub Actions.

**Usage**:
```
Use the ci-cd-setup skill to create a GitHub Actions pipeline for this Node.js project
```

**Expected Output**:
- Complete .github/workflows/ci-cd.yml file
- Build, test, and deploy stages
- Environment-specific configurations
- Secrets documentation
- Deployment procedures

**Example Output**:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm ci
      - run: npm test

  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: npm run deploy
```

### Docker Setup

**Scenario**: You want to containerize your Node.js application.

**Usage**:
```
Use the docker-setup skill to containerize this Express application
```

**Expected Output**:
- Optimized multi-stage Dockerfile
- Docker Compose configuration
- .dockerignore file
- Build and run instructions
- Environment variable documentation
- Production deployment guidelines

## Debugging Skills

### Bug Analyzer

**Scenario**: Your application is throwing an error and you can't figure out why.

**Usage**:
```
Use the bug-analyzer skill to debug this error:
[paste error stack trace]
```

**Expected Output**:
- Root cause analysis
- Step-by-step debugging approach
- Reproduction steps
- Fix recommendation with code
- Prevention strategies
- Tests to add

### Error Handler

**Scenario**: You need to implement proper error handling in your application.

**Usage**:
```
Use the error-handler skill to implement error handling for src/api/
```

**Expected Output**:
- Custom error classes
- Try-catch implementations
- Global error handlers
- Input validation
- Retry logic
- Structured logging examples

**Example Output**:
```javascript
// Custom error classes
class ApplicationError extends Error {
  constructor(message, { cause, code, statusCode } = {}) {
    super(message);
    this.name = this.constructor.name;
    this.cause = cause;
    this.code = code;
    this.statusCode = statusCode;
  }
}

// Error handling middleware
app.use((error, req, res, next) => {
  logger.error('Request error', {
    error: error.message,
    stack: error.stack,
    url: req.url
  });

  res.status(error.statusCode || 500).json({
    error: {
      message: error.message,
      code: error.code
    }
  });
});
```

## Combining Multiple Skills

You can also chain multiple skills together for comprehensive analysis:

```
First, use the security-review skill to check for vulnerabilities,
then use the unit-test-generator skill to create tests for any fixes,
and finally use the code-docs-generator skill to document the changes.
```

## Tips for Best Results

1. **Be Specific**: Reference specific files or directories
2. **Provide Context**: Include error messages, stack traces, or requirements
3. **Ask Follow-up Questions**: Request clarification or additional examples
4. **Iterate**: Use skills multiple times with refined focus
5. **Combine Skills**: Use multiple skills in sequence for comprehensive solutions

## Common Workflows

### New Feature Development
1. `code-refactor` - Clean up existing code
2. `unit-test-generator` - Create tests for new code
3. `integration-test-generator` - Test feature integration
4. `code-docs-generator` - Document the feature
5. `security-review` - Check for security issues

### Bug Fixing
1. `bug-analyzer` - Understand the root cause
2. `error-handler` - Implement proper error handling
3. `unit-test-generator` - Add regression tests
4. `code-quality` - Ensure fix maintains quality

### Code Review Process
1. `security-review` - Check for vulnerabilities
2. `performance-review` - Identify bottlenecks
3. `code-quality` - Assess maintainability
4. `test-coverage-analyzer` - Ensure adequate testing

### Production Deployment
1. `ci-cd-setup` - Automate build and deploy
2. `docker-setup` - Containerize application
3. `api-docs-generator` - Document APIs
4. `error-handler` - Ensure robust error handling
