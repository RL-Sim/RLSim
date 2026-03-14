# Closed Tests

Understanding the philosophy and implementation of closed tests in RLSim.

This documentation is strongly influenced by [Closed Tests Article],
an article by `Simon Willison` that explores the benefits and rationale
behind keeping test cases private.

## Overview

Closed tests are a testing methodology where test cases and their expected outcomes
are kept private and not exposed to the public.
This approach contrasts with open-source testing practices
where tests are typically visible to all users and contributors.

In RLSim, we maintain closed tests to ensure the integrity and reliability
of our traffic simulation validation
without revealing implementation details or test strategies to external parties.

## Why Closed Tests?

### Preventing Test Gaming

When tests are publicly visible,
implementations can be optimized specifically to pass those tests
rather than solving the underlying problem correctly.
Closed tests prevent this by:

- Ensuring implementations are validated against unknown criteria
- Forcing developers to implement robust, general solutions
- Preventing "teaching to the test" scenarios

### Protecting Intellectual Property

Closed tests can contain:

- Proprietary validation methodologies
- Specialized test scenarios developed through research
- Competitive advantages in simulation accuracy
- Confidential test data or edge cases

### Maintaining Test Validity

Public tests can become:

- Reverse-engineered to understand exact requirements
- Circumvented through clever but incorrect implementations
- Outdated as external parties suggest modifications
- Compromised by external contributions that weaken validation

## Implementation in RLSim

### Test Organization

RLSim maintains a dual-test structure:

- **Public tests** - Available in the repository for contributors to validate their work
- **Closed tests** - Maintained separately for final validation and quality assurance

### Validation Process

1. **Development phase** - Contributors use public tests to guide implementation
2. **Submission phase** - Code is submitted for review
3. **Validation phase** - Closed tests verify correctness against hidden criteria
4. **Release phase** - Only code passing closed tests is merged

### Benefits for Traffic Simulation

For traffic rule simulation, closed tests ensure:

- Correct implementation of complex priority rules across different countries
- Proper handling of edge cases and deadlock scenarios
- Accurate vehicle behavior in various traffic conditions
- Compliance with regional traffic regulations

## Transparency and Trust

While tests are closed, the validation process remains transparent:

- Clear feedback is provided when code fails validation
- Developers understand what categories of tests exist
- The reasoning behind test failures is explained
- Improvements to validation criteria are communicated

## Related Concepts

- **Test-Driven Development (TDD)** - Writing tests before implementation
- **Continuous Integration** - Automated testing on every commit
- **Code Review** - Human validation alongside automated tests
- **Regression Testing** - Ensuring new changes don't break existing functionality

## Related Documentation

- [Closed Tests Article]
- [RLSim Architecture]
- [Traffic Rule Specifications]

<!-- Reference Links -->

[Closed Tests Article]: https://simonwillison.net/2026/Feb/25/closed-tests/

[RLSim Architecture]: ../reference/architecture.md

[Traffic Rule Specifications]: ../reference/rules-logic.md
