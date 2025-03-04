---
title: How to use the Spring Boot Starter with the Azure Cosmos DB SQL API
description: Learn how to configure an application created with the Spring Boot Initializer with the Azure Cosmos DB SQL API.
services: cosmos-db
documentationcenter: java
ms.author: karler
ms.date: 01/19/2022
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
---

# How to use the Spring Boot Starter with the Azure Cosmos DB SQL API

This article demonstrates adding the [Azure Spring Boot Starter for Azure Cosmos DB] to a custom application to store data in and retrieve data from your Azure Cosmos DB by using Spring Data and the Cosmos DB SQL API. The article starts by showing you how to create an Azure Cosmos DB using the Azure portal, then shows you how to use [Spring Initializr] to create a custom Spring Boot application that you can use with the Spring Boot Starter.

Azure Cosmos DB is a globally distributed database service that allows developers to work with data using various standard APIs, such as SQL, MongoDB, Graph, and Table APIs. Microsoft's Spring Boot Starter enables developers to use Spring Boot applications that easily integrate with Azure Cosmos DB by using the SQL API.

## Prerequisites

* An Azure subscription; if you don't already have an Azure subscription, you can activate your [MSDN subscriber benefits] or sign up for a [free Azure account].
* A supported Java Development Kit (JDK). For more information about the JDKs available for use when developing on Azure, see [Java support on Azure and Azure Stack](../fundamentals/java-support-on-azure.md).

## Create an Azure Cosmos DB by using the Azure portal

1. Browse to the Azure portal at <https://portal.azure.com/> and select **Create a resource**.

1. Select **Databases**, and then select **Azure Cosmos DB**.

   ![Azure portal, create a resource, search for Azure Cosmos DB.][AZ02]

1. On the **Select API option** screen, select **Core (SQL)**.

   ![Azure portal, create a resource, select API option, Core (SQL) selected.][AZ02-01]

1. On the **Azure Cosmos DB** page, enter the following information:

    * Choose the **Subscription** you want to use for your database.
    * Specify whether to create a new **Resource group** for your database, or choose an existing resource group.
    * Enter a unique **Account Name**, which you will use as the URI for your database. For example: *contosoaccounttest*.
    * Choose **Core (SQL)** for the API.
    * Specify the **Location** for your database.

    When you have specified these options, select **Review + create**, review your specifications, and select **Create**.

    ![Azure portal, Azure Cosmos DB, create Azure Cosmos DB account - Core (SQL).][AZ03]

1. When your database has been created, it is listed on your Azure **Dashboard**, and under the **All Resources** and **Azure Cosmos DB** pages. You can select your database for any of those locations to open the properties page for your cache.

1. When the properties page for your database is displayed, select **Keys** and copy your URI and access keys for your database; you will use these values in your Spring Boot application.

    ![Azure portal, Azure Cosmos DB account, keys section.][AZ05]

## Create a Spring Boot application with the Spring Initializr

Use the following steps to create a new Spring Boot application project with Azure support. As an alternative, you can use the [azure-spring-boot-sample-cosmos](https://github.com/Azure-Samples/azure-spring-boot-samples/tree/main/cosmos/azure-spring-boot-starter-cosmos/cosmos) sample in the [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) repo. Then, you can skip directly to [Build and test your app](#build-and-test-your-app).

1. Browse to <https://start.spring.io/>.

1. Specify the following options:

   * Generate a **Maven** project with **Java**.
   * Specify your **Spring Boot** version.
   * Specify the **Group** and **Artifact** names for your application.
   * Select **11** for the Java version.
   * Add **Azure Support** in the dependencies.

   >[!div class="mx-imgBorder"]
   >![Basic Spring Initializr options][SI01]

   > [!NOTE]
   > 1. The Spring Initializr uses the **Group** and **Artifact** names to create the package name; for example: *com.example.wingtiptoysdata*.
   > 1. The version of Spring Boot may be higher than the version supported by Azure Support. After the project is automatically generated, you can manually change the Spring Boot version to the highest version supported by Azure, which you can find [here][azure-spring-boot-version-matrix].

1. When you have specified the options listed above, select **GENERATE**.

1. When prompted, download the project to a path on your local computer and extract the files.

Your simple Spring Boot application is now ready for editing.

## Configure your Spring Boot application to use the Azure Spring Boot Starter

1. Locate the *pom.xml* file in the directory of your app; for example:

    `C:\SpringBoot\wingtiptoysdata\pom.xml`

    -or-

    `/users/example/home/wingtiptoysdata/pom.xml`

1. Open the *pom.xml* file in a text editor, and add the following to the `<dependencies>` element:

    ```xml
    <dependency>
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-boot-starter-cosmos</artifactId>
        <version>3.10.0</version> 
    </dependency>
    ```

1. Save and close the *pom.xml* file.

## Configure your Spring Boot application to use your Azure Cosmos DB

1. Locate the *application.properties* file in the *resources* directory of your app; for example:

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    -or-

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. Open the *application.properties* file in a text editor, and add the following lines to the file, and replace the sample values with the appropriate properties for your database:

    ```properties
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmos.uri=https://contosoaccounttest.documents.azure.com:443/
    
    # Specify the access key for your database.
    azure.cosmos.key=replace-your-access-key-here
    
    # Specify the name of your database. 
    azure.cosmos.database=contosoaccounttest
    azure.cosmos.populateQueryMetrics=true
    ```

1. Save and close the *application.properties* file.

## Add sample code to implement basic database functionality

In this section you create two Java classes for storing user data, and then you modify your main application class to create an instance of the *User* class and save it to your database.

### Define a base class for storing user data

1. Create a new file named *User.java* in the same directory as your main application Java file.

1. Open the *User.java* file in a text editor, and add the following lines to the file to define a generic user class that stores and retrieve values in your database:

    ```java
    package com.example.wingtiptoysdata;
    
    import com.azure.spring.data.cosmos.core.mapping.Container;
    import com.azure.spring.data.cosmos.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;
    
    @Container(containerName = "mycollection")
    public class User {
        @Id
        private String id;
        private String firstName;
        @PartitionKey
        private String lastName;
        private String address;
    
        public User() {
    
        }
    
        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
        }
    
        public String getId() {
            return id;
        }
    
        public void setId(String id) {
            this.id = id;
        }
    
        public String getFirstName() {
            return firstName;
        }
    
        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }
    
        public String getLastName() {
            return lastName;
        }
    
        public void setLastName(String lastName) {
            this.lastName = lastName;
        }
    
        public String getAddress() {
            return address;
        }
    
        public void setAddress(String address) {
            this.address = address;
        }
    
        @Override
        public String toString() {
            return String.format("%s %s, %s", firstName, lastName, address);
        }
    }
    ```

1. Save and close the *User.java* file.

### Define a data repository interface

1. Create a new file named *UserRepository.java* in the same directory as your main application Java file.

1. Open the *UserRepository.java* file in a text editor, and add the following lines to the file to define a user repository interface that extends the default `ReactiveCosmosRepository` interface:

    ```java
    package com.example.wingtiptoysdata;
    
    import com.azure.spring.data.cosmos.repository.ReactiveCosmosRepository;
    import org.springframework.stereotype.Repository;
    import reactor.core.publisher.Flux;
    
    @Repository
    public interface UserRepository extends ReactiveCosmosRepository<User, String> {
        Flux<User> findByFirstName(String firstName);
    }
    ```

    The `ReactiveCosmosRepository` interface replaces the `DocumentDbRepository` interface from the previous version of the starter. The new interface provides synchronous and reactive APIs for basic save, delete, and find operations.

1. Save and close the *UserRepository.java* file.

### Modify the main application class

1. Locate the main application Java file in the package directory of your application, for example:

    `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

    -or-

    `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

1. Open the main application Java file in a text editor, and add the following lines to the file:

    ```java
    package com.example.wingtiptoysdata;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;
    
    import java.util.Optional;
    
    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {
    
        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);
    
        @Autowired
        private UserRepository repository;
    
        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }
    
        public void run(String... var1) {
            this.repository.deleteAll().block();
            LOGGER.info("Deleted all data in container.");
    
            final User testUser = new User("testId", "testFirstName", "testLastName", "test address line one");
    
            // Save the User class to Azure Cosmos DB database.
            final Mono<User> saveUserMono = repository.save(testUser);
    
            final Flux<User> firstNameUserFlux = repository.findByFirstName("testFirstName");
    
            //  Nothing happens until we subscribe to these Monos.
            //  findById will not return the user as user is not present.
            final Mono<User> findByIdMono = repository.findById(testUser.getId());
            final User findByIdUser = findByIdMono.block();
            Assert.isNull(findByIdUser, "User must be null");
    
            final User savedUser = saveUserMono.block();
            Assert.state(savedUser != null, "Saved user must not be null");
            Assert.state(savedUser.getFirstName().equals(testUser.getFirstName()), "Saved user first name doesn't match");
    
            firstNameUserFlux.collectList().block();
    
            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");
    
            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");
    
            LOGGER.info("findOne in User collection get result: {}", result.toString());
        }
    }
    ```

1. Save and close the main application Java file.

## Build and test your app

1. Open a command prompt and navigate to the folder where your *pom.xml* file is located; for example:

    `cd C:\SpringBoot\wingtiptoysdata`

    -or-

    `cd /users/example/home/wingtiptoysdata`

1. Use the following command to build and run your application:

    ```console
    mvnw clean
    ```

    This command runs the application automatically as part of the test phase. You can also use:

    ```console
    mvnw spring-boot:run
    ```

    After some build and test output, your console window will display a message similar to the following:

    ```console
    INFO 1365 --- [           main] c.e.w.WingtiptoysdataApplication         : Deleted all data in container.
    
    ... (omitting connection and diagnostics output) ...
    
    INFO 1365 --- [           main] c.e.w.WingtiptoysdataApplication         : findOne in User collection get result: testFirstName testLastName, test address line one
    ```

    The above output messages indicate that the data was successfully saved to Cosmos DB and then retrieved again.

## Clean up resources

If you're not going to continue to use this application, be sure to delete the resource group containing the Cosmos DB you created earlier. You can do this from the Azure portal.

## Next steps

To learn more about Spring and Azure, continue to the Spring on Azure documentation center.

> [!div class="nextstepaction"]
> [Spring on Azure](./index.yml)

### More Resources

For more information about using Azure Cosmos DB and Java, see the following articles:

* [Azure Cosmos DB Documentation].

* [Azure Cosmos DB: Create a document database using Java and the Azure portal][Build a SQL API app with Java]

* [Spring Data for Azure Cosmos DB SQL API]

For more information about using Spring Boot applications on Azure, see the following articles:

* [Azure Spring Boot Starter for Azure Cosmos DB]

* [Deploy a Spring Boot application to Linux on Azure App Service](deploy-spring-boot-java-app-on-linux.md)

* [Running a Spring Boot Application on a Kubernetes Cluster in the Azure Container Service](deploy-spring-boot-java-app-on-kubernetes.md)

For more information about using Azure with Java, see the [Azure for Java Developers] and the [Working with Azure DevOps and Java].

The **[Spring Framework]** is an open-source solution that helps Java developers create enterprise-level applications. One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating stand-alone Java applications. To help developers get started with Spring Boot, several sample Spring Boot packages are available at <https://github.com/spring-guides/>. In addition to choosing from the list of basic Spring Boot projects, the **[Spring Initializr]** helps developers get started with creating custom Spring Boot applications.

<!-- URL List -->

[Azure Cosmos DB Documentation]: /azure/cosmos-db/
[Azure for Java Developers]: ../index.yml
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Azure Spring Boot Starter for Azure Cosmos DB]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmos
[free Azure account]: https://azure.microsoft.com/pricing/free-trial/
[Working with Azure DevOps and Java]: https://azure.microsoft.com/services/devops/java/
[MSDN subscriber benefits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[azure-spring-boot-version-matrix]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring#azure-spring-boot

<!-- IMG List -->
[AZ02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ02-01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02-01.png
[AZ03]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ05]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png