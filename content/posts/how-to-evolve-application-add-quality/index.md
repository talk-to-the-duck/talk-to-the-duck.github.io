+++
title = "How to evolve an application to make it more robust and maintainable - Add quality "
description = ""
date = 2023-05-29
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
categories = [
"quality", "test", "CICD", "Sonarqube", "Owasp", "Gradle"
]
tags = [
""
]
series = [
" How to evolve an application to make it more robust and maintainable"
]
images = []
+++

<!--more-->


## Foreword

In our last articles we looked at how to add tests and how to run them automatically via the CI/CD (GitHub Actions).
In this article we'll continue to improve the quality of our project using Sonarqube.



## What is Sonarcloud or Sonarqube
[index.md](..%2FHowTo%2Fhow-to-use-sonarcloud%2Findex.md)


## Integration in java project with Gradle

### Déclare  plugin
* Add plugin

````groovy 
plugins {
id("org.sonarqube") version "4.1.0.3113"
}

````
* Add configuration 
````groovy
sonar {
    properties {
        property "sonar.projectkey", "happraisal"
        property "sonar.projectName", "happraisal"
        property "sonar.host.url", "http://localhost:9000"
        property "sonar.coverage.jacoco.xmlReportPaths", "build/reports/jacoco/all-tests/jacocoAllTestReport.xml"
        property "sonar.sources", "src/main,build.gradle,gradle"
        property "sonar.tests", "src/test,src/integration-test,src/system-test"
        property "sonar.junit.reportPaths", "build/test-results/test,build/test-results/integrationTest,build/test-results/systemTest"
    }
}
````

#### Export results

From intelliJ, via configuration or terminal you can execute :
```` 
clean build jacocoTestReport sonar -Dsonar.login=<<YOUR_ID>> -Dsonar.password=<<YOU_PASSWORD>>

````

If you have created a token you can run 


````
./gradlew clean build jacocoTestReport sonar -Dsonar.login=<<TOUR_TOKEN>>

````

### Multi module
In project with multi-module each module must execute sonar task.


## Tools around Sonar
### Sonar API

Sonar provides some apis to manage project, quality profile, quality gate and others. 

Details can be find [here](https://next.sonarqube.com/sonarqube/web_api/)
You can use tools like postman or curl to use these apis, but the first thing that you must do is to create a token.

To create a token, after you login  go to your profil and in the security tab and create a token.

For some apis you must create a token with administrator authorizations like 
* /api/qualitygates/select
* /api/projects/search



### Sonarlint
Sonarlint is a tool that can be used to analyze your code. 

#### Configuration 
* Add plugin sonarlint in IntelliJ
* Go to Sonarlint settings in Tools/Sonarlint menu
 ![sonarlint-settings.png](sonarlint-settings.png)

* Add  a SonarCloud or Sonarqube connection.  We select Sonarcloud
![sonarlint-create-connexion.png](sonarlint-create-connexion.png)

* Click on the link to connect to Sonarcloud service

![sonarlint-sonarcloud-login.png](sonarlint-sonarcloud-login.png)

* Set token or create one from the link
![sonarlint-sonarcloud-connexion-token.png](sonarlint-sonarcloud-connexion-token.png)

* Select your organization
![sonarlint-sonarcloud-organization-choice.png](sonarlint-sonarcloud-organization-choice.png)

* Select notification configuration
![sonarlint-sonarcloud-configuration-notification.png](sonarlint-sonarcloud-configuration-notification.png)

* Approve the creation 
![sonarlint-sonarcloud-success.png](sonarlint-sonarcloud-success.png)
* bind the project with the connexion
![sonarlint-bind-project-with-connexion.png](sonarlint-bind-project-with-connexion.png)

#### use
In intelliJ there is a sonarlint window
![intellij-sonarlint-window.png](intellij-sonarlint-window.png)

In this window: 

* form current file tab you can analyze the file selected and displayed in IntelliJ
* From report tab you can analyse changed files or all project file



If you have any remarks on the content or the form, you can leave a comment…it is by exchanging that we progress.