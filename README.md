# [projects](http://idugalic.github.io/projects)/serverless-company [![Build Status](https://travis-ci.org/idugalic/serverless-company.svg?branch=master)](https://travis-ci.org/idugalic/serverless-company) [![GitPitch](https://gitpitch.com/assets/badge.svg)](https://gitpitch.com/idugalic/serverless-company/master?grs=github&t=white)


## Table of Contents

  * [FaaS in private clouds](#faas-in-private-clouds)
  * [Spring Cloud Function](#spring-cloud-function)
  * [Building and Running a Function](#building-and-running-a-function)
  * [Slides](#slides)
  * [References and further reading](#references-and-further-reading)

This project is intended to demonstrate best practices for building a serverless web application with help of [Spring](https://github.com/markfisher/spring-cloud-function) platform. 

Serverless architectures refer to applications that significantly depend on third-party services (knows as Backend as a Service or "BaaS") or on custom code that's run in ephemeral containers (Function as a Service or "FaaS"). We will focus on FaaS.

The best known vendor host is currently [AWS Lambda](https://aws.amazon.com/lambda/).

Important attributes of serverless are:

 - No management of server hosts or processes
 - Self auto provision & auto-scale based on load
 - Costs based on actual, precise, usage
 
## FaaS in private clouds

FaaS in private clouds is not quite ready yet. Existing projects need to mature, a lot of considerations still need to be worked.
Existing projects out there are:

- IBM’s [OpenWhisk incubating project at the ASF](https://github.com/openwhisk/openwhisk), 
- [Kubeless](https://github.com/kubeless/kubeless), 
- [Funktion from Fabric8](https://github.com/funktionio/funktion), 
- [Fission from Platform9](https://github.com/fission/fission), 
- [GalacticFog](http://www.galacticfog.com), 
- [IronFunctions](https://github.com/iron-io/functions), 
- [FAAS-for-docker](https://github.com/alexellis/faas)...

## [Spring Cloud Function](https://github.com/markfisher/spring-cloud-function)

Spring Cloud Function provides a new programming model for Spring Boot applications, abstracting away all of the transport details and infrastructure, allowing the developer to keep all the familiar tools and processes, and focus firmly on business logic. It helps you create decoupled functions for serverless hosting providers (like AWS Lambda) or any other runtime target without vendor lock-in.

Spring Cloud Function has 4 main features:

- Wrappers for @Beans of type Function, Consumer and Supplier, exposing them to the outside world as either HTTP endpoints and/or message stream listeners/publishers with RabbitMQ, Kafka etc.
- Compiling strings which are Java function bodies into bytecode, and then turning them into @Beans.
- Deploying a JAR file containing such an application context with an isolated classloader, so that you can pack them together in a single JVM.
- Adapters for AWS Lambda, and possibly other "serverless" service providers.

The @Beans can be Function, Consumer or Supplier (all from java.util), and their parametric types can be String or POJO. A Function is exposed as an HTTP POST if spring-cloud-function-web is on the classpath, and as a Spring Cloud Stream Processor if spring-cloud-function-stream is on the classpath and a spring.cloud.function.stream.endpoint property is configured in the Spring environment. A Consumer is also exposed as an HTTP POST, or as a Stream Sink. A Supplier translates to an HTTP GET, or a Stream Source.


## Building and Running a Function on AWS

### Creating a package

This project uses an [adapter layer for a Spring Cloud Function application onto AWS Lambda](https://github.com/spring-cloud/spring-cloud-function/tree/master/spring-cloud-function-adapters/spring-cloud-function-adapter-aws) - spring-cloud-function-adapter-aws.

The AWS Adapter has a couple of different request handlers you can use like SpringBootRequestHandler, SpringBootStreamHandler, FunctionInvokingS3EventHandler, and so on. If you check the source code of SpringBootRequestHandler, you will see that it instead implements AWS's RequestHandler for us and also propagates the request to our function. The only reason we need to implement it is to specify the type of the input and the output parameters of the function, so AWS can serialize/deserialize them for us.

Build from the command line (and "install" the samples):

```
$ ./mvnw clean package
```

### Deploying a function to AWS (Lambda)

#### AWS console

After a successful build and package, if you navigate to the target directory, you will see two JARs, including one ending with -aws

Let's fire up the AWS Console and navigate to the Lambda service's page. Click on "Create a Lambda function" and select "Blank Function." We don't need any trigger for the function because it will be triggered by API Gateway and we will setup that later on so for now just click on "Next."

On the next page, you need to give a name for your function. I simply gave it "uppercase-function" but you can use anything else. But you need to remember it because it will be required for the setup of API Gateway. For the runtime set "Java 8." Drop the JAR ending with -aws on the upload button. For the lambda function handler, set:

com.idugalic.handler.UppercaseFunctionHandler

TO BE CONTINUED ...

#### AWS CLI

TO BE CONTINUED ...

## Slides

<iframe width='770' height='515' src='https://gitpitch.com/idugalic/serverless-company/master?grs=github&t=white' frameborder='0' allowfullscreen></iframe>


## References and further reading

- [Evolution of Business Logic from Monoliths through Microservices, to Functions](https://read.acloud.guru/evolution-of-business-logic-from-monoliths-through-microservices-to-functions-ff464b95a44d)
- [Deploy your microservice (Amazon Web Services S3, API Gateway and Lambda)](https://www.youtube.com/watch?list=PLVe-2wcL84b8c09AKImmAISiEI6vPefON&v=6ZzGx79Nz4o)
- http://docs.aws.amazon.com/lambda/latest/dg/welcome.html
- https://github.com/kbastani/event-stream-processing-microservices
- http://openwhisk.org/
- https://github.com/krook
- [Functions as a Service (FaaS) with Docker swarm](http://blog.alexellis.io/functions-as-a-service)
- https://medium.com/@pfernandom/serverless-for-private-clouds-or-managing-the-server-for-a-serverless-app-f9321e45a910
- https://www.ibm.com/blogs/bluemix/2017/02/microservices-multi-compute-approach-using-cloud-foundry-openwhisk/
- https://github.com/anaibol/awesome-serverless
- https://github.com/lambci/docker-lambda
- https://github.com/fission/fission
- https://speakerdeck.com/dsyer/spring-cloud-function
- https://github.com/markfisher/spring-cloud-function
- https://dzone.com/articles/run-code-with-spring-cloud-function-on-aws-lambda

