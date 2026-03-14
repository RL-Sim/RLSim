# Public Tests

This directory contains test cases and validation scenarios
that are visible to all contributors in the public repository.

## Purpose

Public tests serve as:

- **Development guidance** - Help contributors understand expected behavior
- **Quality baseline** - Provide basic validation for implementations
- **Learning resource** - Show examples of correct functionality
- **Regression prevention** - Catch breaking changes during development

## Test Organization

- **`unit/`** - Unit tests for individual Rust modules
  - Car movement calculations
  - Traffic rule validation
  - Basic edge case handling

- **`integration/`** - Integration tests
  - Multi-vehicle scenarios
  - WASM-JavaScript communication
  - Basic end-to-end simulation tests

## Running Tests

To run public tests locally:

```bash
cargo test --test '*'
```

To run specific test suites:

```bash
cargo test --test 'unit'
cargo test --test 'integration'
```

## Contributing Tests

When contributing new public tests:

1. Follow existing test patterns and naming conventions
2. Include clear documentation of what is being tested
3. Test both success and failure cases
4. Keep tests focused and independent
5. Ensure tests run reliably without external dependencies

## Important Note

Public tests provide basic validation for development purposes.
Final code validation is performed using closed tests
maintained in a separate private repository.

See [Closed Tests] for detailed information about the validation process.

## Related Documentation

- [Closed Tests] - Understanding the dual-test structure
- [Project Structure] - Overview of test organization
- [Architecture] - System design and testing strategy

<!-- Reference Links -->

[Architecture]: ../../docs/reference/architecture.md
[Closed Tests]: ../../docs/explanation/closed-tests.md
[Project Structure]: ../../docs/reference/project-structure.md
