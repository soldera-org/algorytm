## Frontend

### Technologies/Libraries

- Vue 3.5 (Composition API)
- Bootstrap 5 (with the Phoenix theme)
- Pinia
- VueUse
- Vuelidate
- Lodash
- TypeScript
- HeadlessUI 1.7 for Vue

### TypeScript Code Style

- Use double quotes (`"`) for strings
- Always use semicolons
- Maximum line length is 140 characters
- Use two spaces for indentation
- Use PascalCase for types and interfaces
- Use the import format: `import { Type } from "@/types"`
- Directories should be named in lowercase with dashes (e.g., `components/dashboard/trading-strategy`)
- Prefer named exports for functions
- Use camelCase for variables, functions, methods, and props
- Prefer const over let
- Use descriptive variable names with auxiliary verbs (e.g, isLoading, hasError)

### Programming Style

- Write concise, maintainable, and technically accurate code
- Prefer a functional coding style
- Prefer declarative control flow over imperative control flow
- Prefer iteration and modularization to adhere to DRY principles and avoid code duplication
- Always prefer nullish coalescing (??) over logical OR (||) for providing fallbacks
- **Do not include file header path comments** - Files should not start with comments indicating their file path

#### Declarative Control Flow

Example of declarative control flow to find even numbers from an array (prefer this style):

```ts
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];
// Using built-in array methods
const evenNumbersVanilla = numbers.filter((num) => num % 2 === 0);
// Using lodash (preferred)
const evenNumbersLodash = filter(numbers, (num) => num % 2 === 0);
```

#### Pure Functions

Functions should always return the same output for the same input and have no side effects. They should not modify external state or depend on mutable data.

Exceptions to these guidelines are allowed IF the function's name makes this clear. For example updateAccount, replaceOption etc.

#### Function Composition

Complex operations should be built by combining simpler functions.

For example, instead of a call to Array.reduce with the goal of summing and sorting an array, use the `map` and `sumBy` functions from lodash instead.

If the process is unintuitive or hard to read, use variable names to describe the steps.

#### First-Class Functions

Treat functions as first-class citizens that can be assigned to variables, passed as arguments, and returned from other functions.

For example, if using `@tanstack/vue-query` to describe a mutation operation, you can pass a function to the `onSuccess` option instead of watching the result.

#### Immutability

Data should not be changed after creation. Use const declarations and avoid mutation methods. Instead, create new copies of data when changes are needed.

#### Vue Composables and Reactivity

**Reactivity:**
Vue's reactivity goes against functional programming in some aspects. If a conflict arises between functional programming principles and best practices for writing Vue code, favor best practices for Vue code.

**Composable Functions:**
Functions prefixed with `use` (e.g., `useAccount`, `useCompany`) should be treated as Vue composable functions - follow best practices for writing Vue composable functions and ignore the functional programming rules if a conflict between conventions for writing composables and the rules described in this file comes up.

### Vue Components

#### General Best Practices

- Follow declarative programming patterns
- Adhere to best practices for writing simple, reasonable, maintainable, and easy-to-reason-about frontend code
- Keep communication between components simple and easy to reason about. Separation of concerns is important
- Treat the backend as the source of truth. All state that can be derived from the source of truth should be derived from the source of truth (the backend state)
- Assume that anything fetched from the server can be refetched at any point in time. Components and views should be able to deal with this without additional complex state sync logic
- Where reasonable, keep state in sync with URL query params to aid with browser history management

#### Component-Based Architecture

**Directory Structure:**

```
client/src/
├── dashboard/                    # Main dashboard functionality
│   ├── common/                   # Shared dashboard components
│   │   ├── FollowUpButton.vue   # Common UI elements
│   │   ├── MarketPriceInfoModal.vue
│   │   └── SpotStrategyPicker.vue
│   ├── inventory_management/     # Inventory management section
│   │   ├── common/              # Shared inventory components
│   │   │   ├── companyOrderApi.ts
│   │   │   ├── companyOrderTypes.ts
│   │   │   ├── FileUploadCard.vue
│   │   │   ├── PortfolioItemsTable.vue
│   │   │   └── usePortfolioItemsStore.ts
│   │   ├── company_orders/      # Company orders subsection
│   │   │   ├── BuyGoComponent.vue
│   │   │   ├── SellGoComponent.vue
│   │   │   ├── SendGoComponent.vue
│   │   │   ├── CancelGoStatementComponent.vue
│   │   │   ├── details/         # Order details components
│   │   │   │   ├── CompanyOrderDetailsModal.vue
│   │   │   │   └── OrderDetailsSection.vue
│   │   │   └── types/           # Order-specific types
│   │   │       └── virtualAccountTypes.ts
│   │   ├── accounts/            # Account management
│   │   │   ├── OwnAccountReceiveComponent.vue
│   │   │   ├── ReceiveGoComponent.vue
│   │   │   └── VirtualAccountSelectorComponent.vue
│   │   └── overview/            # Inventory overview
│   │       ├── ChartSelectFilterComponent.vue
│   │       ├── FilterableChartPanel.vue
│   │       └── FilterablePortfolioChartPanel.vue
│   ├── production_strategy/      # Production strategy section
│   │   ├── overview/            # Strategy overview
│   │   │   ├── StrategyOverviewTab.vue
│   │   │   ├── TimelineItem.vue
│   │   │   ├── TimelineRoot.vue
│   │   │   ├── forwards/        # Forward sales subsection
│   │   │   │   ├── ForwardOverviewComponent.vue
│   │   │   │   ├── ForwardSaleScheduleCard.vue
│   │   │   │   ├── ForwardSalesInfo.vue
│   │   │   │   ├── ProductionProjectionBarChart.vue
│   │   │   │   └── StackedBarChart.vue
│   │   │   └── PPA/             # PPA-specific components
│   │   │       ├── PPAStrategyOverview.vue
│   │   │       └── PeriodLimitEditor.vue
│   │   ├── issuance-reporting/  # Issuance reporting
│   │   │   ├── FileUploadModal.vue
│   │   │   ├── IssuanceProductionDataTable.vue
│   │   │   ├── IssuanceReportingTab.vue
│   │   │   └── useIssuanceProductionDataStore.ts
│   │   └── settings/            # Strategy settings
│   │       ├── CountrySelector.vue
│   │       ├── RegistryDevicesSection.vue
│   │       └── StrategyCustomizationTab.vue
│   ├── ConsolidatedDashboardComponent.vue
│   ├── CompanyDataComponent.vue
│   └── BigCompanyComponent.vue
├── admin/                        # Admin functionality
│   ├── commission/              # Commission management
│   │   ├── commission.ts        # Commission API
│   │   └── ReferredCompaniesModal.vue
│   ├── forwards/                # Forward management
│   │   ├── ForwardReservationsDonutChart.vue
│   │   └── ProductionQuarterQuickfill.vue
│   ├── manageAccount.ts         # Account management store
│   └── AdminCompanyManagementComponent.vue
├── money/                        # Financial components
│   ├── admin/                   # Admin financial tools
│   │   └── transactions/        # Transaction management
│   ├── commission/              # Commission components
│   ├── invoice/                 # Invoice management
│   └── SalesTransactionsComponent.vue
├── views/                        # Top-level view components (pages)
│   ├── DashboardView.vue        # Main dashboard page
│   ├── SettingsView.vue         # Settings page
│   ├── admin/                   # Admin views
│   │   ├── AccountsView.vue
│   │   ├── DashboardView.vue
│   │   └── forwards/
│   │       ├── ForwardsView.vue
│   │       └── ForwardReservationsView.vue
│   └── SignUpView.vue
├── router/                       # Vue Router configuration
├── store/                        # Global Pinia stores
├── composables/                  # Global composables
├── components/                   # Global/shared components
│   └── layout/                  # Layout components
│       ├── RootLayout.vue
│       └── UserFacingLayout.vue
├── api/                         # API layer
├── types/                       # Global TypeScript types
└── assets/                      # Static assets
```

### Key Principles for UI-Based Structure

**Organization by User Interface:**

- Structure folders to match the UI layout and user workflow
- Group related functionality by how users interact with features
- Keep shared components within their respective UI sections

**Feature Grouping:**

- `dashboard/` - All main user dashboard functionality
- `admin/` - Administrative tools and interfaces
- `money/` - Financial and transaction-related components
- `views/` - Top-level page components that correspond to routes

**Hierarchical Organization:**

- Use nested folders to represent UI hierarchy (e.g., `dashboard/inventory_management/company_orders/`)
- Place shared components in `common/` folders within their respective sections
- Keep related types, APIs, and stores close to their consuming components

**Route Patterns (matching backend):**

- **Component routes**: `/{component}`
- **Sub-component routes**: `/{component}/{sub-component}`
- **Admin routes**: `/admin/{component}`

**File Naming Conventions:**

- `{Component}Component.vue` - Main component views
- `{Component}ListComponent.vue` - List/table views
- `{Component}FormComponent.vue` - Form components
- `{Component}DetailComponent.vue` - Detail/show views
- `Admin{Component}Component.vue` - Admin components
- `{component}Api.ts` - Component API functions
- `{component}Types.ts` - TypeScript interfaces/types
- `{component}Store.ts` - Main component store

#### Component Structure

- Use Vue 3 Composition API with `<script setup lang="ts">`
- PascalCase filenames with Component.vue suffix
- Script: `<script setup lang="ts">`
- Style: `<style lang="scss" scoped>`

#### Script Setup Organization

Components and views should be written using the Vue Composition API in the `<script setup lang="ts">` syntax.

When organizing code within script blocks, follow this order:

1. Imports (external libraries → Vue core → components/composables → assets/utilities)
2. TypeScript definitions (interfaces/types/enums → prop types)
3. Component setup functions (`defineProps` → `defineEmits` → `defineExpose` → `defineOptions`)
4. State declarations (ref/reactive variables)
5. Computed properties
6. Lifecycle hooks
7. Watchers
8. Methods/event handlers
9. `provide`/`inject` pairs

_Do not start reorganizing the whole file unless instructed to do so._

**Watcher Guidelines**: When implementing a watcher, consider if any alternative is available that would achieve the same result in a more easy-to-debug way.

**Lifecycle Hook Guidelines**: Only put things in the `onMounted` lifecycle hook if it is strictly necessary (i.e., requires some interaction with its DOM). The `onMounted` hook should be used sparingly.

**DOM Manipulation**: If you find yourself needing to directly manipulate and select DOM elements in the code, it is likely a sign that some other alternative approach should be considered.

#### Using Libraries

- Instead of directly using the Vue Router and route modules to interact and update URL queries, heavily prefer using `useQueryManager.ts` unless that module is not capable of achieving the desired results
- Many standard formatting tasks have already been implemented in `formatUtils.ts`, so always check these before implementing your own formatter
- Where possible, use utility functions from Lodash and VueUse. Avoid hand-rolling utility functions unless absolutely necessary. Avoid Lodash cloneDeep unless it's the only option (generally it isn't)

#### Declaring Props

Define props for components using the type-only props declaration. If you need to give props defaults, use `withDefaults`:

```ts
const props = defineProps<{
  foo: string;
  bar?: number;
}>();

const props = withDefaults(
  defineProps<{
    foo: string;
    bar?: number;
  }>(),
  {
    bar: 0,
  }
);
```

If `props` are not used in the `<script>` block of the SFC, you can use `defineProps` without `const props =`.

#### Emitted Events

For communication from child components to parent components, prefer emitting events over passing down functions in props.

Define emitted events for components using the type-only emit declaration:

```ts
const emit = defineEmits<{
  (e: "change", id: number): void;
  (e: "update:strategy", value: string): void;
}>();
```

If `emit` is not used in the `<script>` block of the SFC, you can use `defineEmits` without `const emit =`.

##### Event Naming Scheme

Use a consistent naming scheme for emitted events (`verb:scope`, e.g., `update:strategy`). If no scope is necessary (e.g., input components), then the scope part can be omitted (e.g., `update`).

#### V-Model

Prefer using `v-model` over manually declaring `:value` and handling change events (`@change`, `@input`, etc.).
For components where it makes sense to define props as a data model, use `v-model` in addition/instead of props.

#### Template Best Practices

- Keep props in camelCase in templates
- Use the PascalCase version of component names when using components in templates
- `FontAwesomeIcon` is a globally registered component and can be used from any template block without importing
- When passing props to components and you encounter a situation where the prop name is the same as the variable name, you can use the shorthand syntax: `<Component :someProp>` instead of `<Component :someProp="someProp">`
- Emitting events from the `<template>` block should default to `$emit`
- Never directly define an element's CSS within the `<template>` block. It must be done via references to Bootstrap classes or via classes defined in the custom CSS section

### Styling Priority

1. Bootstrap 5 utility classes and components (always preferred)
2. Custom SCSS ONLY if Bootstrap insufficient

- Strongly favor using Bootstrap 5 utility classes over writing custom CSS. Only write custom CSS if the intended outcome cannot be achieved via Bootstrap 5 utility classes
- Avoid writing custom CSS like the plague. However, if it is unavoidable, use a `<style scoped lang="scss">` in the SFC to write CSS

### Error Handling

- Use a global error handler for unhandled exceptions
- Emit errors to parent components when appropriate
- Provide user-friendly error messages without technical details
- Log errors for debugging (but not sensitive data)
- Use try/catch blocks for asynchronous operations
- Handle edge cases and show appropriate UI feedback
- Implement fallback UI for components that might fail

Example:

```ts
// Use the injected globalErrorHandler
const globalErrorHandler = inject("globalErrorHandler") as (error: any) => void;

try {
  await fetchUserData();
} catch (error) {
  globalErrorHandler(error);
}
```

### Form Validation Errors

- Validate input data before submission
- Display validation errors next to relevant fields
- Use consistent error message styling
- Allow users to fix errors and resubmit

### State Management

- Use Pinia with setup-style store syntax
- Stores should be placed in the `src/client/store/` directory
- For defining state stores, use the setup-style store syntax
- Treat the backend as the source of truth
- Where reasonable, keep state in sync with URL query params

#### Component-Based State Management

**Store Organization:**

```typescript
// money/store/moneyStore.ts
import { defineStore } from "pinia";
import { moneyApi } from "../api/moneyApi";

export const useMoneyStore = defineStore("money", () => {
  const invoices = ref<Invoice[]>([]);
  const loading = ref(false);

  const fetchInvoices = async () => {
    loading.value = true;
    try {
      const response = await moneyApi.listInvoices();
      invoices.value = response.data;
    } finally {
      loading.value = false;
    }
  };

  return {
    invoices: readonly(invoices),
    loading: readonly(loading),
    fetchInvoices,
  };
});

// Sub-component store
// money/portfolio/store/portfolioStore.ts
export const usePortfolioStore = defineStore("portfolio", () => {
  // Portfolio-specific state and actions
});

// Admin store
// money/admin/store/adminMoneyStore.ts
export const useAdminMoneyStore = defineStore("adminMoney", () => {
  // Admin-specific state and actions
});
```

#### API Layer Organization

**Component API Structure:**

```typescript
// money/api/moneyApi.ts
import { goSolidApi } from "@/composables/useGoSolidApi";
import type { Invoice, InvoiceCreateRequest } from "./moneyTypes";

export const moneyApi = {
  // RESTful API functions matching backend routes
  listInvoices: () => goSolidApi.get<Invoice[]>("/money/invoices"),
  createInvoice: (data: InvoiceCreateRequest) =>
    goSolidApi.post<Invoice>("/money/invoices", data),
  getInvoice: (id: number) => goSolidApi.get<Invoice>(`/money/invoices/${id}`),
  downloadInvoice: (id: number) =>
    goSolidApi.get(`/money/invoices/${id}/download`, { responseType: "blob" }),
};

// Sub-component API
// money/portfolio/api/portfolioApi.ts
export const portfolioApi = {
  listTransfers: () => goSolidApi.get("/money/portfolio/transfers"),
  createTransfer: (data: TransferRequest) =>
    goSolidApi.post("/money/portfolio/transfers", data),
};

// Admin API
// money/admin/api/adminMoneyApi.ts
export const adminMoneyApi = {
  listInvoices: () => goSolidApi.get("/admin/money/invoices"),
  getFeeStatistics: () => goSolidApi.get("/admin/money/fees/statistics"),
};
```

#### Migration Strategy for Frontend

**When migrating from flat structure to component-based:**

1. **Identify domain components** (e.g., money, portfolio, market-insights)
2. **Create component directory structure** following the pattern above
3. **Move components to appropriate directories:**
   - Main components: `components/InvoiceComponent.vue` → `money/components/InvoiceComponent.vue`
   - Admin components: `components/AdminInvoiceComponent.vue` → `money/admin/components/AdminInvoiceComponent.vue`
   - Sub-components: Create `money/portfolio/components/TransferComponent.vue`
4. **Update imports** throughout the application
5. **Reorganize API functions** into component-specific API files
6. **Split Pinia stores** by component domain
7. **Update Vue Router** to match new component structure
8. **Create component-specific composables** for shared logic

### Internationalization (i18n)

- **Location**: All translation files should be stored in `client/src/assets/i18n/`
- **Supported languages**: `en`, `et`, `lt`, `lv`
- **Key naming**: Use snake_case for key names with dot notation for hierarchy
- **Source of truth**: English (`en.json`) is the source of truth for all translations
- Maintain consistent formatting across all language files
- Avoid hardcoded text in non-admin components; always use translation keys

#### Variable Interpolation

Use named placeholders for variables in translations:

```json
{
  "welcome_message": "Welcome, {name}!",
  "items_count": "You have {count} item(s) in your cart."
}
```

In code:

```vue
<template>
  <p>{{ $t("welcome_message", { name: user.fullName }) }}</p>
  <p>{{ $t("items_count", { count: cartItems.length }) }}</p>
</template>
```

#### Pluralization

Use appropriate pluralization keys for different quantities:

```json
{
  "cart": {
    "item_count": {
      "zero": "Your cart is empty",
      "one": "You have 1 item in your cart",
      "other": "You have {count} items in your cart"
    }
  }
}
```

### Documentation

- All public functions should use JSDoc with:
  - Description of purpose and behavior
  - `@throws` tag when applicable with error conditions
- Document complex types and interfaces with JSDoc
- Explain non-obvious type properties
- Include examples for complex types when helpful
- Document API endpoints being called
- Document request and response structures
- Include error handling strategies

### Tests

#### Frontend Testing Framework

- **Primary Framework**: Vitest with Vue Test Utils for component testing
- **Test Environment**: jsdom for DOM simulation
- **File Naming**: `{Component}.spec.ts` for components, `{moduleName}.spec.ts` for utilities/stores
- **Location**: `client/tests/` directory following code file path structure

#### Test Structure and Organization

- Place tests in `client/tests/` directory following code file path structure
  - Example: Code file `client/src/store/store.ts` → Test file `client/tests/store/store.spec.ts`
- Use clear and descriptive test names grouped with `describe` blocks
- Isolate test data between `describe` blocks
- Focus on testing actual business logic, avoid testing implementation details

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
