---
Tags: 
Created: 2024-06-24 23:46:17
---
(Links:: [[Software Engineering Processes]])
![[CI CD.png|500]]
# Continuous Integration (CI) & Continuous Delivery and Deployment (CD)

> [!info] Continuous Integration
> Each time a developer performs a change to the project's core repository main branch, an executable version of the system is built and tested

^e72bfb

**Benefits of continuous integration**:
- Faster to find and fix bugs
- Your changes are shared with the whole team
- Might create a "quality culture" in the development team

> [!info] Continuous Delivery
> A simulation of the product's operating environment is created and the executable software version is tested

^cadc42

> [!info] Continuous Deployment
> A new release of the system is made available to users every time a change is made to the project's core repository main branch

^ae233b

**Benefits of continuous deployment**:
- Reduced costs
- Faster problem solving
- Faster customer feedback

## Business reasons against deploying every change
- Incomplete features, but the competitors shouldn't know about it
- Customers may get irritated by many changes
- Synchronization of releases with known business cycles

# Github Actions
The main goal is to **automate** software development tasks and processes. A **workflow** is a configurable automated process that will run one or more jobs. Workflows are defined in a `.yml` file checked in to your repository. Workflows are defined in the `.github/workflows/` directory in the repository. 
A workflow must contain the following basic components:
- One or more **events** that will trigger the workflow
- One or more **jobs**, each of which will execute on a runner machine and run a series of one or more **steps**
- Each step can either **run** a script defined by the developer or a predefined action.
> [!example]+ Compiling a java project with Maven
> ```yaml
> name: Java CI with Maven
> on: push
> jobs: 
> 	my-ci:
> 		runs-on: ubuntu-latest

> 		steps: 
> 		 - name: Check out code
> 		   uses: actions/checkout@v4
> 		
> 		 - name: Set up JDK 17
> 		   uses: actions/setup-java@v3
> 		   with: 
> 			   java-version: '17'
> 			   distribution: 'temurin'
> 		
> 		 - name: Compile with Maven
> 		   run: nvm compile
> ``` 
> - `on` declares which event runs the action
> - `my-ci` is the name of the job
> - Each step can have a name for easier debugging
> 
> Im this case `actions/...` are existing/predefined actions
> The `run` keyword is used for commands that you would run in the terminal

# Software Quality checks
- Executability and functionality are covered by the basic checks (build and tests)

> [!example]- Static Analysis Tools
> | Python      | Java       |
> | ----------- | ---------- |
> | Pylint      | Checkstyle |
> | Pyflakes    | Spotbugs   |
> | pycodestyle | PMD        |
> |             | SonarQube  |


---
References: