# Software Engineering Concepts, Principles, and Best Practices
Repo to take note of the concepts, principles and best practices I've learned (and still learning) during my career as a software engineer.

## Index
- [Index](#index)
- [Third-party Libraries Usage](#third-party-libraries-usage)
- [Logging & Monitoring](#logging-and-monitoring)
- [Code Review Process](#code-review-process)
- [Estimation & Process Planing](#estimation-and-process-planing)
- [Source Code Management](#source-code-management)
- [Database change management](#database-change-management)
- [CI & CD](#ci-and-cd)
- [Code Quality](#code-quality)
- [Performance](#performance)
- [Analysis Before Coding](#analysis-before-coding)
  - [Analyzing requirements](#analyzing-requirements)
  - [Analyzing the design](#analyzing-the-design)
    - [Trade-off Analysis](#trade-off-analysis)
  - [Analyzing the area of impact](#analyzing-the-area-of-impact)

## Third-party Libraries Usage 

- The third-party libraries register is published on the project space (Confluence, Wiki) and regularly maintained by the technical leader or architect.
- Use of a third-party library must comply with its license agreement.
- *The application should not use the library directly, but through the **facade design pattern***. This avoids tight coupling between the third-party library and the application code, so the library can be easily replaced with a newer one.
- Integration tests of the application’s functionality should cover the code of a third-party library. These integration tests ensure that the new version of the library does not break the functionality of the application.
- The development team should use libraries that are actively maintained.
  
</br>

[Back to Index 👆](#index "Go to Index")

## Logging and Monitoring

- When logging a message, the application should use the most appropriate logging level (Trace, Debug, Info, Warning, Error, etc.).
- It should be possible to change the logging level through the config file without redeploying the code.
- Use **structured logging technique** when logging messages so that you can analyze a large amount of logs by writing queries.
- Even actively sending large numbers of logs to the monitoring tool should not significantly impact the performance of the application. Logging calls should be non-blocking. Additionally, they can be batched and sent in a separate thread.
- The monitoring tools like Application Insights, Stackdriver or others is used. The monitoring tool is configured to send notifications when the application exceeds any thresholds.  

</br>

[Back to Index 👆](#index "Go to Index")

## Code Review Process

- The code review has a higher priority than the development task to avoid long-lived pull requests.
- Code review process is automated, not manual. [GitHub](https://github.com/), [Crucible](https://www.atlassian.com/software/crucible), [Review Board](https://www.reviewboard.org/) or other tools are used.
- No more than 300–400 lines of code are sent to the code review. Consider splitting a big functionality into smaller tasks that developers can review separately.
- The reviewer should have an ability to log the time spent on code review.
- The reviewer clearly understands the acceptance criteria of the code he is reviewing.
- The reviewer reviews the code against the code review checklist. The checklist should exist in the project space so that all developers can access and use it.
- Each commit should be reviewed by at least one or two team members before pushing the code to the main branch.

</br>

[Back to Index 👆](#index "Go to Index")

## Estimation and Process Planing

- The entire development team is involved in estimations, not just the technical lead or architect.
- Definition of Done (DoD) for stories is defined and published in the project space. Before doing estimations, the team must reach a consensus on Definition of Done.
- Developers use [3-Point estimation](#https://en.wikipedia.org/wiki/Three-point_estimation) or [PERT Formula](#https://en.wikipedia.org/wiki/Program_evaluation_and_review_technique) to estimate the work, not just single values.
- Estimates should include efforts needed to write unit tests, to pass code review and testing activities.
- **Analogy-based estimation, story points, planning poker, decomposition** and **recomposition** techniques are used during estimation.
- Existing estimates should be reviewed regularly by the team during backlog grooming or sprint planing meetings and refined as needed.
- All team members should be involved in the sprint planning process.
- All developers should be involved in the design phase, not just senior staff.
- Technical leaders do delegation of work in the form of separate tasks.
- Each task should have a written acceptance criteria.

</br>

[Back to Index 👆](#index "Go to Index")

## Source Code Management

- The project source code and all related scripts (database scripts, build or deployment scripts) should be stored in the version control system.
- Each commit to the branch should contain a work item ID from the issue tracking system and a description.
- Developers push changes to the feature branches **few times per day**.
- Each commit must contain functionality that is specific to the only work item.
- Developers should merge pull requests to the main branch as soon as its approved to avoid merge conflicts.
- Binaries that are compiled output from source code or third-party packages should be excluded from source control by configuring the gitignore file.
- Environment-specific data such as connection strings should be separated from the source code and stored in configuration files or environment variables.
- The development team should define branching and merging strategies that meet the needs of the project and describe them in the project space.

</br>

[Back to Index 👆](#index "Go to Index")

## Database change management

- The database schema and reference data (data that is required to run the application) should be stored in the source control.
- The development team should choose between state-based and migration-based approaches according to the project needs.
- Developers should always modify the database through database scripts in source control. If a developer has changed the database directly, he should make the same change in source control.
- Each developer should have their own local database, which they can easily create by running database scripts taken from the repository.

</br>

[Back to Index 👆](#index "Go to Index")

## CI and CD

- The continuous integration workflow should be documented by the technical manager or architect and shared with the team.
- Running the static code analyzer tool should be included in the continuous integration pipeline and cause the build to fail when the code quality metrics are less than specified.
- The continuous integration pipeline should run all unit tests and cause the build to fail if any unit test fails.
- Developers should run unit test locally before committing the code to the repository.
- Build notifications are configured. The build server automatically sends a notification about build status to all interested parties.
- Build artifacts should be versioned using semantic versioning.
- Each build is associated with a corresponding commit from the repository.
- Rollback procedure is defined and configured for failed deployments.

</br>

[Back to Index 👆](#index "Go to Index")

## Code Quality

- The development team should regularly collect and analyze code quality metrics to identify any code issues as quickly as possible.
- All team members should use the same static code analysis tool with the same configuration as the build server.

</br>

[Back to Index 👆](#index "Go to Index")



## Performance

The code should not only work, it should also work fast. This is something that clients may not tell the team when providing acceptance criteria, but it definitely does not mean that any number of milliseconds or seconds will be acceptable for the code to run.

The code is often slow for the following reasons:

- Inappropriate data structure or algorithm was chosen to solve the task.
- The code often “modifies” immutable data types, so it leads to a lot of allocations.
- The application fetches small chunks of data from the database instead of fetching    larger chunks one time, so performance suffers because each call to the database is an expensive operation.

A big number of performance issues may never even appear in test or production environments if you ask yourself immediately after completing coding:

> How fast will my code process a larger dataset?

Usually programmers use local databases for development. The local database schema corresponds to a test or production database. However, the amount of data locally may be less due to the fact that in testing or production environment a lot of data is generated by many numbers of active users. This means that your code can always run fast locally, even with very inefficient algorithms.

[See more details and examples](./examples/Performance/README.md)


</br>

[Back to Index 👆](#index "Go to Index")

## Analysis Before Coding

A programmer often needs to analyze many things before starting to write code. The more complete the analysis, the easier it will be to write the code and the more reliable it will be. Coding itself is easy, the analysis is difficult.

The analysis phase can be broken down into three main parts:

#### Analyzing requirements

You should ensure that the business requirements stated in the ticket make sense to you. Also consider their completeness and talk to your business analyst or product owner if you feel that some edge cases are missing from the description.

#### Analyzing the design

At this stage, you should decide on interfaces, design patterns, integration points of the planned functionality. The larger the functionality, the higher the cost of a mistake made at this stage. Trade-off analysis helps here, because usually a couple of options are available during the design phase, not just one.

#### Trade-off Analysis

Software engineering is a vast and complex topic, so usually almost every problem has several approaches that you take to solve it. For example, there are at least [5 ways to clone an object](https://levelup.gitconnected.com/5-ways-to-clone-an-object-in-c-d1374ec28efa) or [5 ways to implement a repository pattern in C#](https://levelup.gitconnected.com/5-ways-to-implement-repository-pattern-in-c-e12565e4d4a2).

While each of the few available approaches can solve a coding problem, you should not just pick one at random or the most fancy one. You should perform a trade-off analysis because it is the only reliable way to find the best solution for a given problem.

After all the approaches have been identified with pros and cons, it will be much easier to choose the only one without hesitation.

Regular trade-off analysis is what will make you a very confident and experienced software engineer, because this approach will require you to look at the problem from different angles.

[See more details and examples](./examples/Trade-off%20Analysis/README.md)

#### Analyzing the area of impact

Any change to existing code can break other functionality. These are called regression issues. Good engineers usually follow the strategy on how to minimize the risk of regression issues by performing an analysis of the area of impact.

In short, you should find outgoing dependencies (other classes) of the class you are about to modify. The behavior of any outgoing dependency can be affected by your change. After collecting outgoing dependencies, you should understand what functionality of the system they refer to. Then pass the list of functionality to QA engineers, so they can perform a regression testing of all potentially affected locations in the application.


</br>

[Back to Index 👆](#index "Go to Index")