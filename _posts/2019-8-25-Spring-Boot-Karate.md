---
layout: post
title: Spring Boot + Karate - Hello Automation World!
---
Every day many functionalities are developed, pieces of code, lines added to legacy code (or not), small aesthetic improvements or changes that bring another value to the business. In this hyperconnected world, where microservices have become the new stars of the moment, it becomes unthinkable not to have any automated testing mechanism. Such is the motivation of the Karate Framework

## What is Karate?
Is an open-source web-API test-automation framework that can script calls to HTTP end-points and assert that the JSON or XML responses are as expected. Karate's capabilities include being able to run tests in parallel, HTML reports and compatibility with Continuous Integration tools.

## An example
Let's coding a simple Spring Boot Application for get into the Automation testing world
### Requirements
1. Spring Boot 2.1.7 Gradle project. Please generate [project here ](https://start.spring.io/)
2. Karate dependencies for build.gradle
```
dependencies {
    testCompile 'com.intuit.karate:karate-apache:0.6.0'
    testCompile 'com.intuit.karate:karate-junit4:0.6.0'
}
```
### Project structure
![Tree](/images/project.png)

## Features
Karate needs the *.feature files to run the scenarios. For this case we will use the next
```
#src/test/resources/karate/payments.feature
Feature: Payments scenarios

Scenario: Test List of payments response
Given url 'http://www.mocky.io/v2/5d61a8b13200004d008e6137'
When method GET
Then status 200
And match each $ contains {id : '#notnull'}
```
This scenario send a request GET via to a mock server online who response the next
```json
[
    {
        id: 1,
        provider: "BRASPAG",
        currency: "USD",
        ammount_per_unit: "200.1",
        product_id: "123",
        payer_id: "222",
        quantity: 3
    },
    {
        id: 1,
        provider: "MERCADOPAGO",
        currency: "ARS",
        ammount_per_unit: "100.1",
        product_id: "123215",
        payer_id: "222",
        quantity: 1
    }
]
```
The expectations must assert true if the id of every json response is not null

### Test Class
We need only one class to run all scenarios
```java
@RunWith(Karate.class)
@CucumberOptions(features = "classpath:karate")
public class KarateDemoApplicationTests {

    @Test
    public void karateTest() {
    }
}
```
Here the classpath target is the **resources** folder

### Running the tests
If everything is ok you must to see an output like this
![Tests OK](/images/running.png)

### Conclusion
As you can see this implementation is very easy, please checkout this example at LinkedIn

<a href="https://dms.licdn.com/playlist/C4E05AQE3_5uVcI1neA/feedshare-captions-thumbnails-dualWrite-inhouse-mp4_h264_aac_3300k/0?e=1566777600&v=beta&t=zB1DVfXG_pSuPmGhznPNAqg9aswFdgK4X3O9PIcE0ho" rel="Karate at LinkedIn">![Video](/images/karate_video.png)