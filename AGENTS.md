# Repository instructions

- Project is a Maven reactor with two modules: `validators` (Spanish DNI/NIE validation) and `text-analysis` (balanced-bracket validation). Java source/target is 11.
- Run the full verification from the repository root with `mvn test`. The root POM binds PIT mutation coverage to the `test` phase, so this is more than a normal unit-test run and writes mutation reports under module build output.
- Run one module with `mvn -pl <module> test`; run one test class with `mvn -pl <module> -Dtest=<TestClass> test` (for example, `mvn -pl text-analysis -Dtest=BalancedBracketsValidatorTest test`).
- Main code is under `<module>/src/main/java`; JUnit 5 tests are under `<module>/src/test/java`. Module POMs inherit compiler, Surefire, JUnit, and PIT configuration from the root POM.
- Keep generated Maven output (`target/`) out of changes; it is ignored by `.gitignore`.
