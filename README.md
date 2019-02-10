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

Where,
* `configuration.username` is the user to perform the GIT operations
* `configuration.password` is the password of the user
* `configuration.type` is the type of dependency management being used
* `pipeline` setups the stages to execute
* `pipeline.stages` is a map of stage name to `stage`

#### Stage
A stage describe the repositories to update and how to update them. Potentially, only very specific dependencies will want to be updated 
or everything except for a select few.

```json
{
  "updateDependencies": true,
  "updateParent": true,
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

Where,
* `updateDependencies` determines if a repositories dependencies should be updated - if absent, the default value is `false`
* `updateParent` determines if a repositories parent should be updated - if absent, the default value is `false`
* `previousStage` determine which stage needs to be completed first before the current stage can be ran - if absent, the stage will be execute immediately
* `includes` lists the dependencies to only be be updated
* `excludes` lists the dependencies to be excluded from being updated
* `repositories` lists the repositories to update
* `completionStep` specifies the command to run after a repository has been updated