## Documentation Standards

### General Guidelines

#### Comments

Add comments for complex business logic that isn't immediately obvious. Focus on explaining **why**, not what the code does.

##### When to Add Comments

- Complex business logic that isn't immediately obvious
- Non-obvious decisions and algorithm implementations
- Security considerations (prefix with `SECURITY:`)
- Performance optimizations (prefix with `PERF:`)
- Temporary solutions or workarounds (prefix with `WORKAROUND:`)

##### Comment Style

- Add comments to code that requires additional business context or is otherwise unintuitive
- Focus on explaining **why**, not what the code does
- Keep comments concise and to the point
- Use complete sentences with proper punctuation
- Update comments when the code changes
- **Avoid redundant comments** that just repeat what the method signature or code already makes obvious

### What NOT to Comment

Do not add comments that simply restate what is already clear from:

- Method signatures (parameters, return types)
- Variable names and types
- Simple operations or assignments
- Standard patterns or conventions

Example of **bad comments**:

```python
def get_user_by_id(user_id: int) -> User | None:
    """
    Get a user by their ID.

    Args:
        user_id: The ID of the user to retrieve

    Returns:
        User object if found, None otherwise
    """
```

Example of **good comments**:

```python
def get_user_by_id(user_id: int) -> User | None:
    """Get user by ID, checking cache first for performance."""
```

Example:

```
// SECURITY: We use bcrypt instead of a faster hash because security is more important than speed here
const hashedPassword = await bcrypt.hash(password, 10);

// PERF: Memoizing this calculation as it's expensive and called frequently
const memoizedValue = useMemo(() => expensiveCalculation(value), [value]);

// WORKAROUND: setTimeout is needed until API issue #123 is fixed
setTimeout(() => {
  fetchData();
}, 100);
```

### Architecture Documentation

- Complex components should include a brief architecture description
- Document interactions with external systems
- Include diagrams for complex flows (can be added as comments with ASCII or reference to external documentation)
- Document assumptions and constraints

### Code Examples

- Include examples for complex APIs or functions when appropriate
- Make examples concise but complete enough to understand usage
- Ensure examples are runnable and accurate

### Function Documentation

#### TypeScript Functions

- All public functions should use JSDoc with:
  - Description of purpose and behavior
  - `@throws` tag when applicable with error conditions
- Document complex types and interfaces with JSDoc
- Explain non-obvious type properties
- Include examples for complex types when helpful

Example:

```ts
/**
 * Calculates the total price including tax
 *
 * @throws {Error} When price or taxRate is negative
 */
function calculatePriceWithTax(price: number, taxRate: number): number {
  if (price < 0 || taxRate < 0) {
    throw new Error("Price and tax rate must be non-negative");
  }
  return price * (1 + taxRate);
}
```

#### Vue Component Documentation

- Explain any non-obvious behaviors or edge cases
- Document API endpoints being called
- Document request and response structures
- Include error handling strategies

#### Python Functions

- Avoid unnecessary automatic docstrings on methods, let code do the talking.

### Documentation Maintenance

- Update documentation when code changes
- Remove outdated comments and documentation
- Review documentation regularly as part of code reviews
