# Mutant Testing

Small Java examples for practicing unit testing and mutation testing with
[Maven](https://maven.apache.org/) and [PIT](https://pitest.org/).

The project is a Maven reactor containing two independent modules:

- **`validators`** — calculates and validates Spanish personal document
  control characters for DNI and NIE numbers.
- **`text-analysis`** — checks whether an expression has balanced parentheses,
  square brackets, and curly brackets.

## Requirements

Docker Desktop or another Docker-compatible runtime. Java and Maven do not
need to be installed on the host.

## Quick start

Run the complete test suite and generate mutation-coverage reports from the
repository root in the official Maven Docker image:

```bash
docker run --rm \
  -v "$PWD":/workspace \
  -v "$HOME/.m2":/root/.m2 \
  -w /workspace \
  maven:3.9-eclipse-temurin-11 \
  mvn test
```

The `test` phase runs JUnit 5 tests and PIT mutation testing for both modules.
Generated reports are written below each module's `target/` directory.

To test only one module:

```bash
docker run --rm \
  -v "$PWD":/workspace \
  -v "$HOME/.m2":/root/.m2 \
  -w /workspace \
  maven:3.9-eclipse-temurin-11 \
  mvn -pl validators test

docker run --rm \
  -v "$PWD":/workspace \
  -v "$HOME/.m2":/root/.m2 \
  -w /workspace \
  maven:3.9-eclipse-temurin-11 \
  mvn -pl text-analysis test
```

To run one test class:

```bash
docker run --rm \
  -v "$PWD":/workspace \
  -v "$HOME/.m2":/root/.m2 \
  -w /workspace \
  maven:3.9-eclipse-temurin-11 \
  mvn -pl text-analysis -Dtest=BalancedBracketsValidatorTest test
```

Each command mounts the project into the container and reuses the local Maven
cache. Generated reports are written below each module's `target/` directory.

## Usage

### Spanish document validation

`SpanishPersonDocumentValidator` supports the Spanish DNI/NIE control-letter
algorithm. `getControlDigit` receives the document number without its final
control letter; `isValid` receives the complete document identifier.

```java
import demo.validators.SpanishPersonDocumentValidator;

SpanishPersonDocumentValidator validator =
        new SpanishPersonDocumentValidator();

char control = validator.getControlDigit("12345678");
boolean validDni = validator.isValid("12345678Z");
boolean validNie = validator.isValid("X0000001R");
```

NIE prefixes `X`, `Y`, and `Z` are converted to `0`, `1`, and `2`,
respectively, before calculating the control letter.

### Balanced brackets

`BalancedBracketsValidator` recognizes `()`, `[]`, and `{}`. Other characters
are ignored, so expressions may contain ordinary text as well as brackets.

```java
import demo.text.analysis.BalancedBracketsValidator;

BalancedBracketsValidator validator = new BalancedBracketsValidator();

boolean balanced = validator.isBalanced("function() { return [1, 2]; }");
boolean unbalanced = validator.isBalanced("{[}]");
```

## Project layout

```text
.
├── pom.xml
├── validators/
│   └── src/{main,test}/java/
└── text-analysis/
    └── src/{main,test}/java/
```

The project targets Java 11. JUnit 5 provides the unit-test framework, and PIT
is configured at the root so mutation testing is applied during the Maven
`test` phase.

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE).

## Disclaimer

This project is provided for experimentation and educational purposes only. It
is not intended for production use and is provided without warranties of any
kind. Use it at your own risk.

