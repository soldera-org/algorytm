## Backend (Python)

### Code Style

- Use Ruff for formatting and linting (`pixi run lint` or `pixi run lint-fix`)
- Maximum line length is 140 characters
- Use double quotes (`"`) for strings
- Use 4 spaces for indentation
- Follow PEP 8 style guide where not otherwise specified
- Write idiomatic clean python code (follow PEP 20 – The Zen of Python)
- Type hints are **required** for all function parameters and return values
- Use absolute imports

### Naming Conventions

- Use `snake_case` for variables, functions, methods, and modules
- Use `PascalCase` for classes
- Use `UPPERCASE_WITH_UNDERSCORES` for constants
- Prefix private methods and attributes with a single underscore (`_`)
- **Application vs Generator**: Use Application instead of Generator for all new code
- **Preferred Classes**: Use Application instead of Generator for all new code

### Function and Method Design

- Keep functions and methods small and focused on a single responsibility
- Use descriptive names that indicate what the function does
- Use keyword arguments for clarity when calling functions with multiple parameters
- Prefer return statements over modifying parameters
- Files should read like a book - public interface at the top, implementation details at the bottom

### File Organization

**Imports**: Always place all file imports at the top of the file. DO NOT DO INLINE IMPORTS.

**Import grouping**: Group imports in the following order with blank lines between groups:

1. Standard library imports
2. Third-party imports
3. Local application imports

**File structure**: Organize file contents in this order:

1. Imports (grouped as above)
2. Constants and module-level variables
3. Type definitions (Pydantic models, TypedDict, etc.)
4. Public functions and classes
5. Private functions and classes (prefixed with `_`)

**Context managers**: Always use context managers for file operations, database connections, and other resources

**DRY principle**: Extract common functionality into shared helper functions rather than duplicating code

Files should read like a book - public interface at the top, implementation details at the bottom.

### Documentation

- Avoid unnecessary automatic docstrings on methods, let code do the talking.

### Validation

- Use Pydantic for request/response models, configuration objects, complex data structures, and API input validation
- Naming conventions:
  - Request models: `...Request`
  - Response models: `...Response`

### Layered Architecture

- **Routes/Controllers**: Handle request processing, input validation, and response formatting
- **Services**: Contain business logic and orchestrate operations
- **Models**: Define database entities and data access patterns

Each layer has a specific responsibility, and dependencies should flow downward (controllers depend on services, services depend on models).

### API Endpoints

Use kebab-case for endpoint paths following RESTful conventions

- Routes are defined directly on controller methods using Flask decorators
- Follow RESTful conventions for resource operations:
  - GET: Retrieve resource(s)
  - POST: Create a new resource
  - PUT: Update a resource completely
  - DELETE: Remove a resource
- All API responses should use consistent structure: `{"data": {/* resource or resources */}}`
- All request data (params, body, etc) must be validated using Pydantic models within the controller
- Authentication required unless explicitly public
- Supported roles: `super_admin` (access to all resources), `user` (limited access based on ownership)

#### API Security and Authorization

- Implement proper authorization checks based on user roles
- Supported roles include:
  - `super_admin`: Access to all resources and operations
  - `user`: Limited access based on resource ownership

#### API Error Handling

- Only return success responses in controllers
- All errors are handled in global handler for all controllers in app.py
- No need to create separate 400, 500 or other types of json responses in controllers

#### Response Format

- **Preferred**: Return Pydantic models directly: `jsonify(response.model_dump()), 200`
- **Avoid**: Nested data wrapper: `jsonify({"data": response.model_dump()})` - creates awkward `response.data.data` patterns on frontend
- Use consistent response structure across endpoints for better developer experience

### Controller Design

- Controllers should be thin and focused on HTTP concerns
- Delegate business logic to services
- Handle request validation and response formatting
- Use appropriate HTTP status codes and consistent response structures
- Follow RESTful principles where appropriate

#### Blueprint Organization

- Use Flask Blueprints to organize routes by feature
- Keep blueprint registration centralized in a single location in app.py

### Directory Structure

**Component-Based Architecture (Current Standard):**

```
app/
├── {component}/ # Domain-specific components (e.g., money/, portfolio/, market_insights/)
│ ├── api/ # API layer for this component (controllers and validators)
│ │ ├── {component}_controller.py # Main component route handlers
│ │ └── {component}_validators.py # Request/response validation (inherits from service signatures)
│ ├── admin/ # Admin functionality for this component
│ │ ├── api/ # Admin API layer
│ │ │ ├── admin_{feature}_controller.py # Admin route handlers
│ │ │ └── admin_{feature}_validators.py # Admin request/response validation
│ │ ├── models/ # Admin-specific database models
│ │ ├── jobs/ # Admin background jobs
│ │ └── {feature}_service.py # Admin services for specific features (contains Pydantic models)
│ ├── {sub_component}/ # Sub-components under main component with full structure
│ │ ├── api/ # Sub-component API layer
│ │ │ ├── {sub_component}_controller.py # Sub-component controllers
│ │ │ └── {sub_component}_validators.py # Sub-component validation
│ │ ├── models/ # Sub-component database models
│ │ ├── jobs/ # Sub-component background jobs
│ │ └── {sub_component}_service.py # Sub-component services (contains Pydantic models)
│ ├── models/ # Component-specific database models
│ ├── jobs/ # Background jobs for this component
│ └── {feature}_service.py # Core component services (contains Pydantic models)
├── extensions/ # Flask extensions setup
├── services/ # Shared services with no domain-specific context
├── utils/ # Global utility functions and helpers
└── models/ # Legacy global models (being phased out)
```

**Legacy Structure (Deprecated - Being Phased Out):**

```
app/
├── api/ # DEPRECATED: Old API structure, avoid for new features
│ ├── feature1/
│ │ ├── controller.py
│ │ └── service.py
│ └── feature2/
```

**Route Structure - RESTful Best Practices:**

- Routes are defined directly on controller methods using Flask decorators
- Follow RESTful conventions with proper HTTP methods
- Use plural nouns for collections, avoid verbs in URLs

**Route Patterns:**

- **Component routes**: `/{component}/{resources}/{optional resource_id}`
- **Sub-component routes**: `/{component}/{sub_component}/{resources}/{optional resource_id}`
- **Admin routes**: `/admin/{component}/{resources}/{optional resource_id}`

**RESTful Examples:**

- `GET /money/invoices` - List invoices (component)
- `POST /money/invoices` - Create invoice (component)
- `GET /money/invoices/<int:invoice_id>` - Get specific invoice (component)
- `GET /money/invoices/<int:invoice_id>/download` - Download invoice file (component)
- `GET /portfolio/transfers` - List portfolio transfers (sub-component)
- `POST /portfolio/transfers` - Create transfer (sub-component)
- `GET /companies/<int:company_id>/portfolios` - Get company portfolio (nested resource)
- `GET /admin/money/invoices` - Admin list invoices (admin)
- `GET /admin/money/fees/statistics` - Admin fee statistics (admin)
- `GET /admin/portfolio/transfers` - Admin portfolio transfers (admin)

### Service Location Conventions

**Component-Based Services (Preferred):**

- **Component Services**: Place domain-specific business logic in `app/{component}/` directory

  - Core component functionality: `app/{component}/{feature}_service.py`
  - Sub-component services: `app/{component}/{sub_component}/{sub_component}_service.py`
  - Admin services: `app/{component}/admin/{feature}_service.py` (specific admin features)

- **Service Method Signatures**: Services should contain Pydantic models as their method signatures

  - Define input/output models directly in the service file
  - Use these models to ensure type safety and validation at the service layer
  - Controller validators should inherit from and reuse these service method signatures

- **Controller Validators**: Located in `app/{component}/api/` directory

  - Use and inherit from service method signatures for consistency
  - Add additional controller-specific validation if needed
  - Maintain separation between service logic and HTTP concerns

- **Global Shared Services**: Place cross-component reusable logic in `app/services/` directory
  - These services have no domain-specific context
  - They implement utility logic that can be used across different components
  - Examples: email services, file management, authentication utilities

**Legacy Services (Deprecated):**

- **Feature-specific Services**: `app/api/{feature}/` - AVOID for new development
  - These should be migrated to the component-based structure

### Migration Strategy

**When refactoring from app/api/\* to component-based structure:**

1. **Identify the domain component** (e.g., money, portfolio, market_insights)
2. **Create component directory structure:**
   ```
   app/{component}/
   ├── api/
   │   ├── {component}_controller.py
   │   └── {component}_validators.py
   ├── admin/
   │   ├── api/
   │   │   ├── admin_{feature}_controller.py
   │   │   └── admin_{feature}_validators.py
   │   ├── models/
   │   ├── jobs/
   │   └── {feature}_service.py
   ├── {sub_component}/
   │   ├── api/
   │   │   ├── {sub_component}_controller.py
   │   │   └── {sub_component}_validators.py
   │   ├── models/
   │   ├── jobs/
   │   └── {sub_component}_service.py
   ├── models/
   ├── jobs/
   └── {component}_service.py
   ```
3. **Move controllers and update routes:**
   - Component: `app/api/{feature}/controller.py` → `app/{component}/api/{component}_controller.py`
   - Admin: Create `app/{component}/admin/api/admin_{feature}_controller.py`
   - Sub-components: Create `app/{component}/{sub_component}/api/{sub_component}_controller.py`
   - **Replace routes.py references with direct route decorators**
   - Update route paths to follow RESTful conventions:
     - Component: `/api/{feature}` → `/{component}/{resources}`
     - Admin: `/api/admin/{feature}` → `/admin/{component}/{resources}`
     - Sub-component: Create `/{component}/{sub_component}/{resources}`
   - Use proper HTTP methods (GET, POST, PUT, PATCH, DELETE) instead of action-based routes
   - Remove route definitions from `app/routes.py`
4. **Extract business logic to services:**
   - Move complex logic from controllers to `*_service.py` files
   - Define Pydantic models in service files for method signatures
   - Create validators in `api/*_validators.py` files that inherit from service signatures
5. **Update imports across the codebase**
6. **Register new blueprints in app.py**
7. **Phase out routes.py**: Remove route constants and use direct Flask decorators

### Service Design

- Services should implement business logic and workflows
- Services should be stateless and use dependency injection when possible
- Services can use other services and repositories
- Keep services focused on a specific domain concern
- Avoid direct Flask dependencies in services when possible

#### Service Method Signatures with Pydantic Models

- **Define Models in Service Files**: Each service should contain Pydantic models that define the input and output signatures of its methods
- **Type Safety**: Use these models to ensure type safety and validation at the service layer
- **Reusability**: Controller validators should inherit from and reuse these service method signatures
- **Consistency**: This ensures consistency between service interfaces and API contracts

Example:

```python
# app/money/invoice_service.py
from pydantic import BaseModel
from decimal import Decimal
from datetime import datetime

class InvoiceCreateRequest(BaseModel):
    amount: Decimal
    customer_id: int
    description: str

class InvoiceResponse(BaseModel):
    id: int
    amount: Decimal
    customer_id: int
    description: str
    created_at: datetime

def create_invoice(request: InvoiceCreateRequest) -> InvoiceResponse:
    # Service logic here
    pass
```

```python
# app/money/api/invoice_validators.py
from app.money.invoice_service import InvoiceCreateRequest, InvoiceResponse

# Controller validators inherit from service signatures
class CreateInvoiceRequest(InvoiceCreateRequest):
    # Add controller-specific validation if needed
    pass

class CreateInvoiceResponse(InvoiceResponse):
    # Add controller-specific response formatting if needed
    pass
```

### Presenters

- Transform internal models to external representations for API responses
- Keep stateless and separate from business logic
- Use clear method names (e.g., `to_response`, `to_dto`)

### Jobs

- Handle background processing and scheduled tasks
- Design for idempotency with proper error handling and logging
- Maintain single responsibility and loose coupling

### Validators

- Use Pydantic with naming convention: `...Request` and `...Response`
- Implement custom validators for complex business rules
- Provide clear error messages separate from business logic

### Database

Use SQLAlchemy 2.0 query style and data model patterns.

#### Database Models

- Document model classes with their table relationships
- Document columns with their data types and constraints
- Explain indexes and unique constraints
- Document any special behaviors or lifecycle hooks

Example:

```python
class PortfolioItem(db.Model, SoftDeleteMixin):
    """
    Links a single Certificate to a single Portfolio while tracking how many MWh (count)
    from that certificate are logically assigned to that portfolio.
    """
    __tablename__ = "portfolio_item"
    __table_args__ = (
        Index("ix_portfolio_item_certificate_id", "certificate_id"),
        Index("ix_portfolio_item_portfolio_id", "portfolio_id"),
        Index("ix_portfolio_item_forward_reservation_id", "forward_reservation_id"),
    )

    id: Mapped[int] = mapped_column(primary_key=True)  # noqa: A003
    certificate_id: Mapped[int] = mapped_column(ForeignKey(Certificate.__tablename__ + ".id"), nullable=False)
    portfolio_id: Mapped[int] = mapped_column(ForeignKey(Portfolio.__tablename__ + ".id"), nullable=False)

    # Partial or total count of the certificate in this portfolio
    count: Mapped[int] = mapped_column(Integer, nullable=False)

    # Portfolio specific fields
    forward_reservation_id: Mapped[int | None] = mapped_column(ForeignKey(ForwardReservation.__tablename__ + ".id"), nullable=True)

    # Metadata
    updated_at: Mapped[datetime] = mapped_column(DateTime(timezone=True), default=lambda: datetime.now(UTC), nullable=False)

    # Relationships
    portfolio: Mapped[Portfolio] = relationship("Portfolio", back_populates="items")
    certificate: Mapped[Certificate] = relationship("Certificate")
    forward_reservation: Mapped[ForwardReservation | None] = relationship("ForwardReservation")
```

#### Queries

- Use SQLAlchemy 2.0 query style (`db.session.execute(select(...))`)
- Import `select`, `update`, `delete` from sqlalchemy
- Use fluent style with method chaining for complex queries
- Use appropriate result methods like `scalar_one()`, `scalar_one_or_none()`, `scalars().all()`
- Prefer SQLAlchemy filtering over Python filtering for efficiency
- IMPORTANT: when writing Python code that queries database avoid introducing N+1 queries. Instead of fetching records inside loops try to fetch records before looping or try use joins to include associated records.

### Error Handling

- Surface errors to global handler
- Rollback database changes on errors
- Provide user-friendly error messages without exposing technical details
- Always log stack traces for errors
- Don't log sensitive data (passwords, tokens, etc.)
- Include contextual information in logs (user ID, request ID, etc.)
- Use different log levels appropriately (debug, info, warning, error)
- Structure logs for easier parsing and analysis

#### Controller Error Handling Pattern

**IMPORTANT**: Do not use try-except blocks in controllers. We have global error handlers that catch server errors and log them automatically. Using try-except in controllers suppresses these errors and prevents proper logging.

**Bad Practice** (suppresses global error handlers):

```python
@app.route("/api/data", methods=["GET"])
def get_data():
    try:
        # Parse and validate query parameters
        query_params = QueryParams(**dict(request.args))

        # Get data through service
        service = DataService()
        result = service.get_data(query_params)

        return jsonify(result.model_dump(mode="json")), 200

    except ValidationError as e:
        return jsonify({"error": "Invalid query parameters", "details": e.errors()}), 400
    except ValueError as e:
        return jsonify({"error": str(e)}), 400
```

**Good Practice** (allows global error handlers to work):

```python
@app.route("/api/data", methods=["GET"])
def get_data():
    # Parse and validate query parameters
    query_params = QueryParams(**dict(request.args))

    # Get data through service
    service = DataService()
    result = service.get_data(query_params)

    return jsonify(result.model_dump(mode="json")), 200
```

The global error handlers will automatically:

- Catch and log ValidationError, ValueError, and other exceptions
- Return appropriate HTTP status codes
- Log stack traces with proper context
- Handle database rollbacks when needed

### Tests

- Place tests in the `tests/` directory
- Name files `test_{module_name}.py`
- Group tests by functionality to different test classes within the same test file
- Use clear and descriptive test names that indicate what is being tested
- Include both positive tests (expected behavior) and negative tests (error handling)
- **Inherit from `BaseDBTestCase` for tests requiring database access**
- **Implement `setUp` to prepare test state and `tearDown` for cleanup**
- **Mock external dependencies to isolate tests**
- **Use `unittest` library for test structure**
- Create test data in `setUp` methods
- Use factories (test_factory.py) or fixtures for complex test data
- Isolate test data between test cases
- Don't rely on existing database state
- Mock external dependencies (APIs, services, etc.) to isolate tests
- Use `unittest.mock` for mocking
- Be specific about what to mock to avoid over-mocking
- Verify mock calls when behavior is important
- Write tests for bug fixes to prevent regressions
- Focus on testing behavior, not implementation details
- Patterns for Python source file and test files:
  ```
  {
    pattern = "app/(.*)/(.*).py$",
    target = "tests/%1/test_%2.py",
  },
  {
    pattern = "tests/(.*)/test_(.*).py$",
    target = "app/%1/%2.py",
  }
  ```
  - If you are asked to add tests for a file and corresponding test file doesn't exist then create it
  - Example:
    - source file: app/services/foobar.py
    - test file: tests/services/test_foobar.py
