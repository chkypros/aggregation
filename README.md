This project was created to have a look into how the -pl option works.

# Project Structure
    aggregation
    ├── README.md
    ├── moduleA
    │   ├── pom.xml
    │   └── src
    │       ├── main/java/dummy
    │       │   └── AppA.java
    │       └── test/java/dummy
    │           └── AppATest.java
    ├── moduleB
    │   ├── pom.xml
    │   └── src
    │       ├── main/java/dummy
    │       │   └── AppB.java
    │       └── test/java/dummy
    │           └── AppBTest.java
    ├── moduleC
    │   ├── pom.xml
    │   └── src
    │       ├── main/java/dummy
    │       │   └── AppC.java
    │       └── test/java/dummy
    │           └── AppCTest.java
    └── pom.xml

# Module Relations
- All modules `A`,`B`,`C` are sub-modules of the root `aggregation` module
- Modules `A` and `B` define `aggregation` module as their parent
- Module `B` defines `C` as its depedendency

# Building the modules
Builds are all invoked from the aggregation dir

## Building without -pl
Command: `mvn clean package`
Result: Modules built: aggregation, A, C, B

## Building with -pl
Command: `mvn clean package -pl .`
Result: Modules built: aggregation

## Building with -pl and -am
Command: `mvn clean package -pl . -am`
Result: Modules built: aggregation

## Building with -pl, -am and -amd
Command: `mvn clean package -pl . -am -amd`
Result:
  * Modules identified to be built: aggregation, A, B
  * Build fails: `Failed to execute goal on project moduleB: Could not resolve dependencies for project dummy:moduleB:jar:1.0-SNAPSHOT: Could not find artifact dummy:moduleC:jar:1.0-SNAPSHOT`

# Notes
* Seems that child projects are identified as dependants (thus -amd picks them up).
* Seems that sub-modules are not identified as dependencies in the aggregate module (thus -am doesn't pick them up)
