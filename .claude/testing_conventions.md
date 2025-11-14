## Testing Guidelines

### Backend Testing

#### Test Structure and Organization

- Place tests in the `tests/` directory
- Name test files with the pattern `test_{module_name}.py`
- Group tests by functionality to different test classes within the same test file
- Use clear and descriptive test names that indicate what is being tested
- Include both positive tests (expected behavior) and negative tests (error handling)

#### Test Base Classes

- Inherit from `BaseDBTestCase` for tests requiring database access
- Implement `setUp` to prepare test state
- Use fixtures to create reusable test data
- Mock external dependencies to isolate tests

#### Test Data Management

- Create test data in `setUp` methods
- Use ([test_factory.py](mdc:tests/utils/test_factory.py)) to create models in the tests
- Use fixtures for complex test data
- Isolate test data between test cases
- Don't rely on existing database state

#### Mocking

- Mock external dependencies (APIs, services, etc.)
- Use `unittest.mock` for mocking
- Be specific about what to mock to avoid over-mocking
- Verify mock calls when behavior is important

#### Coverage and Quality

- Write tests for bug fixes to prevent regressions
- Focus on testing behavior, not implementation details

### Frontend Testing

#### Test Structure and Organization

- Place tests in `client/tests/` directory. Follow code file path structure.
  Example:
  Code file: client/src/store/store.ts
  Test file: client/tests/store/store.spec.ts
- Use clear and descriptive test names. Group relevant tests with `describe` blocks.
- Isolate test data between `describe` blocks.
- Focus on testing actual business logic. Avoid testing implementation details.

#### Frontend Testing Framework

- **Primary Framework**: Vitest with Vue Test Utils for component testing
- **Test Environment**: jsdom for DOM simulation
- **File Naming**: `{Component}.spec.ts` for components, `{moduleName}.spec.ts` for utilities/stores
- **Location**: `client/tests/` directory following code file path structure

#### Test Setup and Configuration

- Global test setup in `client/tests/setup.ts`
- Mock external dependencies appropriately
- Use proper Pinia setup with `setActivePinia(createPinia())` for store tests
- Mock Vue Router and other Vue ecosystem dependencies
- Configure global mocks for common components (e.g., FontAwesome icons)

#### Component Testing Best Practices

- Test component behavior, not implementation details
- Use factory functions for creating test data
- Mock external API calls and services
- Test both success and error scenarios
- Verify computed properties and reactive state changes
- Test event emission and prop validation

#### Store Testing Patterns

- Test store state initialization
- Test all actions and their side effects
- Test computed properties and getters
- Test error handling in store actions
- Mock external dependencies (API calls, router)
- Verify URL synchronization for stores that manage query parameters

#### Mocking Guidelines

- Mock external dependencies at module level using `vi.mock()`
- Create realistic mock data that matches production types
- Use factory functions for generating test data
- Mock Vue ecosystem dependencies (router, i18n) appropriately
- Avoid over-mocking - mock only what's necessary for isolation

#### Test Commands

- Frontend Vitest tests: `cd client && yarn test:unit:vitest:run`
- Frontend Vitest watch mode: `cd client && yarn test:unit:vitest`
- Frontend Vitest UI: `cd client && yarn test:unit:vitest:ui`
- Backend single test: `pixi run test tests/path/to/test_file.py::TestClass::test_method -v`
- Backend all tests: `pixi run test`
