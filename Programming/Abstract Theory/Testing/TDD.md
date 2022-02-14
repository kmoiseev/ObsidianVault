### Tests in TDD should GUIDE the code
---
## TDD Cycle
- **Write failing unit test**
- **Make the test pass**
- **Refactor to make it as simple as possible**
- **Repeat**

---
#### Developers are liberate to do their tasks when the code is covered and driven by tests

---
### How TDD helps us with both design and implementation?

- make us clarify the acceptance criteria for the next piece of work (design)
- encourages us to write loosely coupled components so they can easily be tested in isolation and combined together at higher levels (design)
- add an executable description of what the code does (design)
- new entries in the regression suite (impl)
- detects errors while context is fresh (impl)
- let us know we have done enough discouraging "gold plants" (design)

---
**Never write new functionality without failing test**

---
#### Acceptance test exercises the new functionality that is going to be built
Wherever possible, an acceptance test should exercise the system end-to-end without directly calling its internal code. An end-to-end test interacts with the system only from the outside: through its user interface, by sending messages as if from third-party systems, by invoking its web services, by parsing reports, and so on. As we discuss in Chapter 10, the whole behavior of the system includes its interaction with its external environment.

An automated build, usually triggered by someone checking code into the source repository, will: 
- check out the latest version; 
- compile and unit-test the code; 
- integrate and package the system; 
- perform a production-like deployment into a realistic environment; 
- and, finally, exercise the system through its external access points. 

---