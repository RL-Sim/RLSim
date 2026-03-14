# Closed Tests

This directory contains private test cases and validation scenarios
that are not visible in the public repository.

## Why This Directory Is Empty

The closed tests are maintained in a separate private repository
accessible only to a select group of maintainers and core contributors.

This approach ensures:

- **Test integrity** - Prevents implementations from being optimized specifically to pass known tests
- **Intellectual property protection** - Keeps proprietary validation methodologies confidential
- **Robust validation** - Ensures code is validated against unknown criteria
- **Quality assurance** - Maintains comprehensive test coverage without public exposure

## How Closed Tests Work

1. **Development phase** - Contributors use public tests to guide implementation
2. **Submission phase** - Code is submitted for review via merge request
3. **Validation phase** - Maintainers run closed tests against the code
4. **Feedback phase** - Clear feedback is provided if validation fails
5. **Merge phase** - Only code passing closed tests is merged to main branch

## For Contributors

When you submit a merge request:

- Your code will be validated against closed tests
- You will receive clear feedback if validation fails
- The reasoning behind test failures will be explained
- You understand what categories of tests exist (unit, integration)
- Improvements to validation criteria are communicated

## For Maintainers

Access to closed tests requires:

- Core maintainer status in the RLSim project
- Explicit permission from the project lead
- Adherence to confidentiality agreements
- Responsibility for test maintenance and updates

## Related Documentation

For detailed information about closed testing methodology,
see the [Closed Tests Explanation] documentation.

<!-- Reference Links -->

[Closed Tests Explanation]: ../../docs/explanation/closed-tests.md
