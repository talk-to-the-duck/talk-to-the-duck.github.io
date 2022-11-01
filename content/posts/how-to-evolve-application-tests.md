+++
title = "How to evolve an application to make it more robust and maintainable - Setting up the test"
description = ""
date = 2022-02-27T18:18:31+01:00
featured = false
draft = false
comment = true
toc = true
reward = true
pinned = false
categories = [
  "quality", "test"
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


# How to evolve an application to make it more robust and maintainable - Setting up the test

## Foreword
In the introductory article of this series (link) we presented the different problems of our application (add link). The first one I propose to solve in this article is the addition of tests.
The purpose of adding tests is not only to increasing the code coveragecover the code and pass a quality gate; it is there to validate the business rules and allow refactoring with less risk.
In this article we will look at how to add tests to an application that does not have any.
We will discuss unit tests, integration tests and system tests


## Definitions of the different types of tests
A unit test is a test that only checks the method of a class or a class. External classes, often called "collaborators", that might be called will be mocked. We will place unit tests mainly on services that must contain business code, i.e. code with added value.
An integration test is a test that aims to verify the correct integration of certain parts of a process and not the process as a whole. We will concentrate our integration tests on endpoints and repositories.
A system test is a black-box test that verifies that the entire use case works.

![test_pyramid.png](images/posts/how-to-evolve-application/test-pyramid.png "image_tooltip")


In a case such as that of our application, where no tests exist, it is preferable to start with a system test to quickly verify the correct operation and non-regression of a functionality, and then to increase the unit tests and integration tests.


## Unit testing
Since our application is in Java we will use as libraries for the tests:
Junit5, Mockito and AssertJ.


### Setting up the test
The dependencies are to be added in the build.gradle but for readability reasons I prefer to create a unit-test.gradle file which will contain all the necessary configurations.
This file is called by the build.gradle file

**Location of the unit-test.gradle file**

![unit-test-gradle-file-location](images/posts/how-to-evolve-application/unit-test-gradle-file-location.png "image_tooltip")


**Contents of the unit-test.gradle file**
``` groovy
dependencies {

   testImplementation group: 'org.junit.jupiter', name: 'junit-jupiter', version: '5.8.2'
   testImplementation group: 'org.mockito', name: 'mockito-junit-jupiter', version: '4.2.0'
   testImplementation group: 'org.assertj', name: 'assertj-core', version: '3.21.0'
   testImplementation group: 'org.mockito', name: 'mockito-core', version: '4.2.0'
}

```


**In the build.gradle file**
``` groovy
apply from: "gradle/unit-test.gradle"
```



### Implementation of a test
The rules for writing a unit test :  
* Each test must be created in the test directory
* Each test class is in the same package as the class it tests
* The names of the test methods are in snack_case ( (cf [https://en.wikipedia.org/wiki/Snake_case](https://en.wikipedia.org/wiki/Snake_case))
* The number of things tested should be limited, ideally to 1.


## **Example of a test**
```

@Test
void should_add_question_to_form() {
 // given
 var formId = UUID.fromString("18b8f2e2-f41c-4ca4-a007-c328390cd099");
 var savedForm = new Form();
 var existingQuestion = new QuestionAnswer(formId, "What is your first name", null, savedForm);
 var questions = new HashSet<QuestionAnswer>();
 questions.add(existingQuestion);
 savedForm.setQuestions(questions);
 savedForm.setId(formId);

 BDDMockito.when(formRepository.getById(formId)).thenReturn(savedForm);

 var questionToSave =
     new QuestionAnswer(null, "What is your objectives for the next year", null, null);

 // when
 formService.createQuestion(formId, questionToSave);

 // then
 var questionAnswerArgumentCaptor = ArgumentCaptor.forClass(QuestionAnswer.class);

 BDDMockito.then(questionAnswerRepository).should().save(questionAnswerArgumentCaptor.capture());
 var actualQuestions = questionAnswerArgumentCaptor.getValue();
 BDDSoftAssertions.thenSoftly(
     softly -> {
       softly.then(actualQuestions).isNotNull();
       softly.then(actualQuestions)
               .extracting("form.questions", Assertions.as(InstanceOfAssertFactories.COLLECTION))
               .hasSize(2);
     });
}

```



### Code details
The most important points are
* The use of mocks to simulate the behaviour of methods of other classes that are called

``` java
BDDMockito.when(formRepository.getById(formId)).thenReturn(savedForm);

```

* The use of ArgumentCaptor to catch and then validate the form passed to the save method of the repository

``` java
 var questionAnswerArgumentCaptor = ArgumentCaptor.forClass(QuestionAnswer.class);

 BDDMockito.then(questionAnswerRepository).should().save(questionAnswerArgumentCaptor.capture());
     var actualQuestions = questionAnswerArgumentCaptor.getValue();

```

* The use of SoftAssertions to be able to check several things. Unlike validation via the Assertions class, which will raise an error on the first error, SoftAssertions will test all conditions and raise errors afterwards.

``` java
 BDDSoftAssertions.thenSoftly(
     softly -> {
       softly.then(actualQuestions).isNotNull();
       softly.then(actualQuestions)
               .extracting("form.questions", Assertions.as(InstanceOfAssertFactories.COLLECTION))
               .hasSize(2);
     });
```


## Integration testing
### Setting up the test

To set up the integration tests we will create a directory at the same level as the test and main directories.

![integration-test-folder.png](images/posts/how-to-evolve-application/integration-test-folder.png "image_tooltip")

The separation of unit tests and integration tests has the advantage of allowing us to distinguish which type of tests we want to run and when; this will be useful when setting up the CI/CD.
The configuration will be done in the same way as for unit tests via a separate gradle file.

**Location of the integration-test.gradle file**
![unit-test-gradle-file-location.png](images/posts/how-to-evolve-application/unit-test-gradle-file-location.png "image_tooltip")


**Content of the integration-test.gradle file**
``` groovy
sourceSets {
    integrationTest {
        java {
            compileClasspath += sourceSets.main.output
            runtimeClasspath += sourceSets.main.output
            srcDir file('src/integration-test/java')
        }
        resources.srcDir file('src/integration-test/resources')
    }
}

configurations {
    integrationTestImplementation.extendsFrom implementation
    integrationTestRuntimeOnly.extendsFrom runtimeOnly
}

dependencies {
    integrationTestImplementation group: 'org.springframework.boot', name: 'spring-boot-starter-test'
    integrationTestImplementation group: 'org.mockito', name: 'mockito-core', version: '4.2.0'
    integrationTestImplementation group: 'org.mockito', name: 'mockito-inline', version: '4.2.0'
}

tasks.register('integrationTest', Test) {
    description = 'Runs integration tests.'
    group = 'verification'

    testClassesDirs = sourceSets.integrationTest.output.classesDirs
    classpath = sourceSets.integrationTest.runtimeClasspath
    shouldRunAfter test
    useJUnitPlatform()
}

check.dependsOn integrationTest
```

**Dans le fichier build.gradle**

```
apply from: "gradle/unit-test.gradle"
apply from: "gradle/integration-test.gradle"

```



### Implementation of a test
As the integration tests takeare longer, we will limit them to controllers and repositories; services, as we have seen above, are tested by unit tests.
At the level of controllers, the objectives will be to verify that the endpoints respond correctly, i.e. we will verify :
* the validity status returned.
* error feedback due to invalid parameters or in the body of the request

The services called in the controllers will be mocked, and the calls to the service methods can be validated.
This way of testing makes us understand why it is necessary to limit the business code in the controllers.
At the repository level, we will check the validity of the requests, concentrating initially on the most complex requests.


**Example of a controller integration test**
``` java
@WebMvcTest(controllers = InterviewController.class)
public class InterviewControllerITest {

 @Autowired Jackson2ObjectMapperBuilder mapperBuilder;

 @Autowired private MockMvc mockMvc;

 @MockBean private InterviewService interviewService;

 @Test
 @DisplayName("Should get all interviews")
 void should_get_persons() throws Exception {
   // when
   mockMvc
       .perform(
           MockMvcRequestBuilders.get("/interviews")
               .contentType(MediaType.APPLICATION_JSON)
               .accept(MediaType.APPLICATION_JSON))
       .andExpect(MockMvcResultMatchers.status().isOk());

   // then
   BDDMockito.then(interviewService).should().findAll();
 }


```


**Example of a repository integration test**
``` java

@DataJpaTest
@ActiveProfiles("it")
public class FormRepositoryITest {
   @Autowired
   private FormRepository formRepository;

   @Test
   void should_create_form() {
       // given
       var questionAnswer = new QuestionAnswer(null, "What do you think about your work this year", null, null);
       var formToSave = new Form(null, Set.of(questionAnswer));

       // when
       var actualForm = formRepository.save(formToSave);

       // then
       var idNotNull = new Condition<Form>((Form form) -> form.getId() != null, "form id not null");
       var asOneQuestion = new Condition<Form>((Form form) -> form.getQuestions().size() == 1, "form has only one question");
       BDDAssertions.then(actualForm)
               .as("Check if a new form has been saved.")
               .has(idNotNull)
               .has(asOneQuestion);
   }


```



### Code details
#### end-point testing
For endpoint tests we annotate the test classes with @WebMvcTest, which allows us to load a minimal environment in order to test only the controller defined in the annotation.
``` java
@WebMvcTest(controllers = InterviewController.class)
```


The spring MockMvc class allows endpoint testing without loading the entire application context.
``` java
   mockMvc
       .perform(
           MockMvcRequestBuilders.get("/interviews")
               .contentType(MediaType.APPLICATION_JSON)
               .accept(MediaType.APPLICATION_JSON))
       .andExpect(MockMvcResultMatchers.status().isOk());
```


In order to test only the endpoint (return code, returned object ...) we mock the service which is called with the @MockBean annotation of Sspring
``` java
 @MockBean private InterviewService interviewService;
```




#### Repositories testing
Integration tests between a repository and the database are done with the @DataJpaTest annotation ` `
When testing the registration of an object, it is possible to check the generated identifier of the object in return
For methods like find, findAll, findId we can use the @Sql annotation to populate the database before testing. This is useful when the database at startup is empty. Here I have not done this but it is quite possible to start the tests with a pre-initialised database.

``` java
@Sql(scripts = {"/sql_scripts/initialize_form.sql"} )
```


## System testing
### Setting up the test
As with the integration tests, the system tests are in a separate directory; the configuration is in the file system-test.gradle.

![system-test-folder.png](images/posts/how-to-evolve-application/system-test-folder.png "image_tooltip")


**Location of the system-test.gradle file**

![system-test-gradle-file-location.png](images/posts/how-to-evolve-application/system-test-gradle-file-location.png "image_tooltip")


**Contents of the system-test.gradle file**
``` groovy
sourceSets {
   systemTest {
       java {
           compileClasspath += sourceSets.main.output
           runtimeClasspath += sourceSets.main.output
           srcDir file('src/system-test/java')
       }
       resources.srcDir file('src/system-test/resources')
   }
}

configurations {
   systemTestImplementation.extendsFrom implementation
   systemTestRuntimeOnly.extendsFrom runtimeOnly
}

dependencies {
   systemTestImplementation group: 'org.springframework.boot', name: 'spring-boot-starter-test'
   systemTestImplementation group: 'io.rest-assured', name: 'rest-assured', version: '4.4.0'
   systemTestImplementation group: 'org.mockito', name: 'mockito-core', version: '4.2.0'
   systemTestImplementation group: 'org.mockito', name: 'mockito-inline', version: '4.2.0'


}

```


**In the build.gradle file**
``` groovy

apply from: "gradle/unit-test.gradle"
apply from: "gradle/integration-test.gradle"
apply from: "gradle/system-test.gradle"

systemTest {
  shouldRunAfter integrationTest
}

```


### Implementation of a test
``` java

@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class PostInterviewTest {
 @Autowired Jackson2ObjectMapperBuilder mapperBuilder;

 @LocalServerPort

 private Integer port;

 @BeforeAll
 public static void setup() {
   RestAssured.baseURI = "http://localhost";
 }
 @Test
 @Sql(scripts = "/sql_scripts/init_database.sql")
 void should_create_interview() throws JsonProcessingException {
   var manager =
       new Person(UUID.fromString("8a37adbc-0c29-491a-8b6f-f10a4219fb91"), null, null, true);
   var employee =
       new Person(UUID.fromString("3556c835-3bcd-420e-bb6e-a8b7c0bb139d"), null, null, false);
   var form = new Form(UUID.fromString("df9b2aad-d32e-469e-b434-be71a5531a35"), new HashSet<>());
   var interview = new Interview(null, Instant.now(), manager, employee, form);

   RestAssured.given()
           .header("Content-type", "application/json")
           .and()
           .port(port)
       .body(mapperBuilder.build().writeValueAsString(interview))
       .when()
           .post("/interviews")
       .then()
           .statusCode(HttpStatus.SC_CREATED);
 }
}

```

### Code details
The points to note in this test are:
* The use of the SpringBootTest annotation to load the entire spring context.
* The use of RestAssured to test endpoints.


## Summary
* There are three types of test bases: unit tests which test a method and only the content of the method, integration tests which validate the correct integration between two layers (endpoint calls, repository/database connections) and finally system tests which validate the correct operation of a functionality.
* For an existing API without tests, it is possible to start by setting up system tests on the most critical endpoints and then gradually adding integration and unit tests.
* For a new API it is best to start with unit testing and progress to system testing

If you have any comments on the content, the form you can leave a comment...it is by exchanging that we progress.

Written by : Emmanuel Quinton
Review by : Daniele Cremonini
