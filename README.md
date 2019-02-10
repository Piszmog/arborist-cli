# Arborist CLI
CLI for managing dependency trees.

## Problem
Microservice ecosystems can grow to become very complex with a lot of dependencies. When a new feature is introduced or a bug is fix, 
it can occur at the lowest levels of the dependency tree. To get the change to bubble up to the application level potentially requires releasing 
updates for all libraries and parents that depend on the base library.

This requires a lot of time and the probability of missing a dependency grows are the number of libraries and parents to change 
increases.

![image](images/problem.png)

Above is a general example of a dependency tree for a single application. It is a lot of work to ensure everything is up-to-date.

### Dependency Management Solutions
Fortunately, most languages have a concept of dependency management.

Java has Maven and Gradle, .NET has NuGet, Go has Go Modules, and ect...

These tools can be used to ensure the correct dependencies are being used by an application or a library. But as that dependency tree grows large, 
these tools are not always able to handle a coordinated version update thru the whole tree.

## Solution
The Arborist CLI tool is meant to help coordinate updates to an application's, library's, or parent's dependencies no matter how deep 
and complex the tree has grown.

The CLI allows the ability to create stages for a pipeline to cleanup and update the dependency tree.

### Pipeline
A pipeline is made up of `N` number of stages. A stage is made up of the libraries/parents to update. A stage can be configured to depend 
on another stage to complete before being ran.

```json
{
  "configuration": {
    "username": "",
    "password": "",
    "type": "maven"
  },
  "pipeline": {
    "stages": {
      "${named stage}" : {
        ...
      }
    }
  }
}
```

#### Stage
```json
{
  "updateDependencies": true,
  "updateParents": true,
  "previousStage": "",
  "includes": [
    {
      "groupId": "",
      "artifactId": ""
    }
  ],
  "excludes": [
    {
      "groupId": "",
      "artifactId": ""
    }
  ],
  "repositories": [
    ""
  ],
  "completionStep": "mvn deploy"
}
```