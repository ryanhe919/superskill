# Performance Code Review

You are performing a comprehensive performance code review. Your goal is to identify performance bottlenecks and optimization opportunities.

## Focus Areas

1. **Algorithm Efficiency**
   - Analyze time complexity
   - Review space complexity
   - Identify inefficient loops
   - Check for unnecessary computations

2. **Database Performance**
   - Review query efficiency
   - Check for N+1 query problems
   - Verify proper indexing
   - Examine connection pooling

3. **Memory Management**
   - Identify memory leaks
   - Check for excessive memory usage
   - Review object lifecycle
   - Verify proper resource cleanup

4. **Frontend Performance**
   - Analyze bundle size
   - Review rendering performance
   - Check for unnecessary re-renders
   - Verify lazy loading implementation

5. **Caching**
   - Review caching strategies
   - Identify caching opportunities
   - Check cache invalidation logic
   - Verify cache hit rates

6. **Async Operations**
   - Review async/await usage
   - Check for blocking operations
   - Verify parallel execution opportunities
   - Examine promise handling

## Review Process

1. Analyze code for performance issues
2. Measure potential impact of each issue
3. Prioritize optimization opportunities
4. Provide specific improvement suggestions
5. Include benchmarking recommendations

## Output Format

For each finding:
- **Category**: [Algorithm/Database/Memory/Frontend/Caching/Async]
- **Location**: [file:line_number]
- **Issue**: [Description of performance problem]
- **Impact**: [Expected performance improvement]
- **Current Complexity**: [Big O notation if applicable]
- **Recommendation**: [How to optimize]
- **Optimized Code**: [Improved implementation]

Begin the performance review now.
