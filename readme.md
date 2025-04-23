# 29CM Frontend Campus - Code Conventions

## Table of Contents

1. [General Principles](#1-general-principles)

   - [Modern JavaScript Features](#modern-javascript-features)
   - [Code Organization](#code-organization)
   - [Component Design](#component-design)

2. [Naming Conventions](#2-naming-conventions)

   - [General](#general)
   - [Files & Directories](#files--directories)
   - [Code Formatting](#code-formatting)

3. [API Design](#3-api-design)

   - [API Management](#api-management)

4. [Quality Standards](#4-quality-standards)

   - [Common Test Cases](#common-test-cases)
   - [Code Quality](#code-quality)

5. [Next.js Best Practices](#5-nextjs-best-practices)

   - [Page and Component Structure](#page-and-component-structure)
   - [Performance Optimization](#performance-optimization)
   - [Data Fetching](#data-fetching)
   - [Performance](#performance)

6. [BAD/GOOD practices](#6-badgood-practices)

   - [Component Naming](#component-naming)
   - [Filtering Props](#filter-out-unnecessary-props-when-possible)
   - [Error Boundaries](#implement-proper-error-boundaries)
   - [JSX Formatting](#wrap-jsx-tags-in-parentheses-when-they-span-more-than-one-line)
   - [Self-Closing Tags](#always-self-close-tags-that-have-no-children)
   - [Image Optimization](#optimize-images-and-assets)
   - [Loading States](#implement-proper-loading-states)
   - [Image Alt Text](#add-meaningful-alt-text-to-images)
   - [Semantic HTML](#use-semantic-html)
   - [Memoization](#memoize-expensive-calculations)
   - [TypeScript Types](#use-any-type-unnecessarily)
   - [Component Nesting](#nest-components-too-deeply)
   - [Text Management](#hardcode-text-use-constants)
   - [TypeScript Warnings](#ignore-typescript-warnings)

7. [Frontend Workflow Checklist](#7-frontend-workflow-checklist)
   - [Estimation Process](#estimation-process)
   - [Transfer Meeting](#transfer-meeting)
   - [Task Implementation](#task-implementation)
   - [Bug Fixing](#bug-fixing)

## 1. General Principles

### Modern JavaScript Features

- Use optional chaining (`?.`) and nullish coalescing (`??`) operators
- Implement proper async/await patterns with error handling
- Use array/object destructuring for cleaner code
- Leverage array methods (map, filter, reduce) over imperative loops
- Use template literals for string interpolation

### Code Organization

- Create utility hooks for reusable logic
- Use barrel exports (index.ts files) to simplify imports
- Implement proper error handling patterns
- Use meaningful variable names that indicate types and purposes

### Component Design

- Create small, reusable components with focused responsibilities
- Use composition over inheritance
- Optimize for readability and maintainability

## 2. Naming Conventions

### General

- Use descriptive names that explain purpose (prefer clarity over brevity)
- `camelCase` for variables, functions, methods
- `PascalCase` for components, types, interfaces, classes
- `UPPER_SNAKE_CASE` for constants
- `kebab-case` for CSS classes and file names (except components)

### Files & Directories

- React components: `PascalCase.tsx` (e.g., `Button.tsx`)
- Utility files: `camelCase.ts` (e.g., `formatDate.ts`)
- Test files: `[filename].test.tsx` or `[filename].spec.tsx`
- Style files: `[filename].module.css` or `[filename].styles.ts`

### Code Formatting

- Use provided ESLint and Prettier configurations

## 3. API Design

### API Management

- Use HTTP client libraries (Axios or fetch) based on project requirements
- Manage API state with TanStack Query or client-specified state management solution

## 4. Quality Standards

### Common Test Cases

- Loading states
- Error states
- Empty states
- Boundary/edge cases
- Accessibility
- Responsive behavior

### Code Quality

- Run ESLint before each commit
- Maintain code coverage targets

## 5. Next.js Best Practices

### Page and Component Structure

- Use server components for data fetching when possible
- Keep client components lightweight by moving logic to server components
- Implement error boundaries at the page level

### Performance Optimization

- Use `next/image` for all images to leverage automatic optimization
- Implement proper image sizing and use responsive images
- Enable static generation where possible with `getStaticProps`/`getStaticPaths`
- Use Incremental Static Regeneration for dynamic content that changes infrequently
- Implement proper code splitting with dynamic imports

### Data Fetching

- Use React Server Components for initial data fetching
- Implement React Query for client-side data fetching
- Cache API responses appropriately using TanStack Query's caching mechanisms
- Use `next/dynamic` with `{ ssr: false }` for components that should only render client-side

### Performance

- Memoize expensive calculations with `useMemo`
- Optimize callback functions with `useCallback`
- Implement proper debouncing and throttling for user inputs
- Profile and optimize render performance with React DevTools

## 6. BAD/GOOD practices

- Component Naming

```tsx
// bad
import Footer from "./Footer/Footer";

// bad
import Footer from "./Footer/index";

// good
import Footer from "./Footer";
```

- Filter out unnecessary props when possible

```tsx
// bad
const Component = (props) => {
  const { irrelevantProp, ...relevantProps } = props;
  return <WrappedComponent {...props} />;
};

// good
const Component = (props) => {
  const { irrelevantProp, ...relevantProps } = props;
  return <WrappedComponent {...relevantProps} />;
};
```

- Implement proper error boundaries

```tsx
// Bad
// No error handling
const ProductPage = () => {
  const { data } = useProductData();
  return <ProductDisplay product={data} />;
};

// Good
// With error boundary
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <ErrorFallback />;
    }
    return this.props.children;
  }
}

const ProductPage = () => (
  <ErrorBoundary>
    <ProductContent />
  </ErrorBoundary>
);
```

- Wrap JSX tags in parentheses when they span more than one line

```tsx
  // bad
  render() {
    return <MyComponent variant="long body" foo="bar">
            <MyChild />
          </MyComponent>;
  }

  // good
  render() {
    return (
      <MyComponent variant="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  }

  // good, when single line
  render() {
    const body = <div>hello</div>;
    return <MyComponent>{body}</MyComponent>;
  }

```

- Always self-close tags that have no children

```tsx
// bad
<Foo variant="stuff"></Foo>

// good
<Foo variant="stuff" />
```

- Optimize images and assets

  ```tsx
  // Bad
  <img src="/large-unoptimized-image.jpg" />;

  // Good
  import Image from "next/image";

  <Image
    src="/image.jpg"
    width={800}
    height={600}
    quality={85}
    priority={false}
    alt="Product image"
  />;
  ```

- Add meaningful alt text to images

  ```tsx
  // Bad
  <img src="/product.jpg" alt="" />

  // Good
  <img src="/product.jpg" alt="Blue denim jacket with button closure, size medium" />
  ```

- Memoize expensive calculations

  ```tsx
  // Bad
  const ProductGrid = ({ products }) => {
    // This will recalculate on every render
    const sortedProducts = products.sort((a, b) => a.price - b.price);

    return (
      <div className="grid">
        {sortedProducts.map((product) => (
          <ProductItem key={product.id} product={product} />
        ))}
      </div>
    );
  };

  // Good
  const ProductGrid = ({ products }) => {
    // Memoized calculation, only runs when products change
    const sortedProducts = useMemo(() => {
      return [...products].sort((a, b) => a.price - b.price);
    }, [products]);

    return (
      <div className="grid">
        {sortedProducts.map((product) => (
          <ProductItem key={product.id} product={product} />
        ))}
      </div>
    );
  };
  ```

- Use `any` type unnecessarily

  ```typescript
  // Bad
  function processData(data: any) {
    return data.map((item: any) => item.value);
  }

  // Good
  interface DataItem {
    id: string;
    value: number;
  }

  function processData(data: DataItem[]) {
    return data.map((item) => item.value);
  }
  ```

- Nest components too deeply

  ```tsx
  // Bad
  const App = () => (
    <Layout>
      <Header>
        <Navigation>
          <NavItems>
            <NavItem>
              <NavLink>
                <NavText>Home</NavText>
              </NavLink>
            </NavItem>
          </NavItems>
        </Navigation>
      </Header>
    </Layout>
  );

  // Good
  const NavLink = ({ children }) => <a className="nav-link">{children}</a>;

  const NavItem = ({ children }) => <li className="nav-item">{children}</li>;

  const Navigation = () => (
    <nav>
      <ul>
        <NavItem>
          <NavLink>Home</NavLink>
        </NavItem>
      </ul>
    </nav>
  );

  const App = () => (
    <Layout>
      <Header>
        <Navigation />
      </Header>
    </Layout>
  );
  ```

- Ignore TypeScript warnings

  ```typescript
  // Bad
  // @ts-ignore
  const result = someFunction(invalidArgument);

  // Good
  type ValidArgument = string | number;

  function someFunction(arg: ValidArgument) {
    // Implementation
  }

  const validArgument: ValidArgument = "valid input";
  const result = someFunction(validArgument);
  ```

## 7. Frontend Workflow Checklist

### Estimation Process

- [ ] FE lead receives list of tasks for the sprint
- [ ] FE lead distributes tasks to team members
- [ ] Team members create estimation documents for assigned tasks
- [ ] Team reviews all estimations together to:
  - [ ] Identify new technologies (pros/cons)
  - [ ] Identify shared UI/logic components
  - [ ] Prioritize tasks to avoid blocking
  - [ ] Identify affected areas requiring updates

### Transfer Meeting

- [ ] Dev confirms task details with BA during meeting
- [ ] Address questions from estimation file Q&A section
- [ ] Note QC comments in estimation file
- [ ] Update estimation file if requirements change
- [ ] FE lead compiles and sends time estimates to PM and BA

### Task Implementation

- [ ] Create branch from dev following coding conventions
- [ ] Implement task according to estimation file
- [ ] Mark completed tasks in estimation file before PR
- [ ] Submit PR for review by FE lead
- [ ] Address PR comments if any
- [ ] FE lead merges PR and deploys to dev environment for QC testing

### Bug Fixing

- [ ] Update bug ID in estimation file
- [ ] Create branch from dev following coding conventions
- [ ] Fix bug and notify team if common components are affected
- [ ] Submit PR for review
- [ ] Address PR comments if any
- [ ] FE lead merges PR and deploys to dev environment
- [ ] After QC approval, notify team members with affected tasks to update code
