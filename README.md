# Cashdrawer Device API Documentation

This documentation provides an overview of the Cash Drawer Device API, a RestAPI developed using the Micronaut framework with Java 11 and RXJava. The API allows consumers to set up and interact with a cash-drawer device by exposing HTTP endpoints. It incorporates Open Telemetry for tracing, monitoring, and logging and utilizes OpenAPI Specification to expose the service capabilities.

## Table of Contents

- [Problem Statement](#problem-statement)
- [Current State](#current-state)
- [Solution and Architecture](#solution-and-architecture)
- [Techstack](#techstack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Endpoints](#endpoints)
- [Testing](#testing)
- [Monitoring and Tracing](#monitoring-and-tracing)
  
# Problem Statement

The Cash Drawer Device API is developed to add JavaPOS support to an existing application. It enables users to integrate cashdrawer functionality into their applications seamlessly. The API is built using Micronaut, a lightweight framework designed for microservices and serverless applications. RXJava is utilized to handle asynchronous operations efficiently.

The API exposes a set of HTTP endpoints that allow users to configure and control the cashdrawer device. Additionally, a simulator is provided as a device service to demonstrate the capabilities of the API through end-to-end Behavior-Driven Development (BDD) tests.

Open Telemetry is incorporated to provide tracing, monitoring, and logging capabilities for better observability of the API. Grafana and Prometheus are used as telemetry tools to visualize and analyze performance metrics.

The API documentation is generated using Swagger UI, which provides a user-friendly interface to explore and interact with the API.

## Current State

The current McDonald's POS architecture has a client application consisting of three main components. The first component is the UI, which handles the user interface. The second component is the driver host, which is responsible for hosting multiple device adapters for device communication through adapters. These adapters communicate with various devices. The third component is the message communication layer, facilitating communication with the server.

Within the driver host, there are different implementations for OPOS (OLE for Retail POS) and non-OPOS devices. Adapters are used to load drivers, which directly communicate with the devices. For OPOS, there is a DSL OPOS application that utilizes an adapter, which, in turn, uses a driver to load a common control object. This object then communicates with the device.

One of the limitations of this architecture is that it only works on Windows due to the tight coupling with the Windows OS. The applications are developed in C++, with additional dependencies, such as the message communication implementation.

In summary, the current McDonald's POS architecture involves a client application with a UI, a driver host with adapters for device communication, and a message communication layer. The architecture has specific implementations for OPOS and non-OPOS devices, using adapters and drivers. However, it is limited to Windows and depends on C++ and other components.

## Solution and Architecture

The complete POS architecture is mainly composed of two parts, i.e., client and server. The previous server part is represented at the top right corner. Mcdonald's is currently in the process of migrating some of its server logic to microservices. We are helping in making the client side also a microservice and making it OS-independent. 

So, to achieve this, we used the JavaPOS implementation. Mcdonald's already had the implementation in OPOS. Our team made an improvised version in OPOS and C# language, overcoming a few setbacks in their architecture. To achieve device compatibility and OS independency, the team has implemented JavaPOS.

In this implementation, we made a CashDrawerAPI containing a controller that can control the CashDrawerService and interact with the service object using the JavaPOS control object. Vendors usually provide this JavaPOS service object, but we made a simulator here. And previously, the message hub client was used for communication, but here, we used an HTTP client. This is how we improved their client side using JavaPOS and made it OS-independent.

## TechStack

The Cash Drawer Device API uses a micronaut framework with gradle build.
It is written in Java language along with the rx java library for reactive programming. Open telemetry for tracing through jaeger, metrics through prometheus and grafana and logs through logger factory have also been incorporated. Open api implementation using the swagger ui to display the functioning of endpoints and docker is used for containerization. For unit as well as bdd testing junit5 and mockito is used. Gitlab is used for the implementation of ci pipeline.


## Prerequisites

Before getting started with the Cash Drawer Device API, ensure that you have the following prerequisites:
1. Java 11 Development Kit (JDK) installed on your system
2. Gradle build tool installed (https://gradle.org/install/)
3. A text editor (e.g., IntelliJ IDEA) for code editing and project management

## Installation

To set up the Cash Drawer Device API, follow these steps:

1. Clone the repository from the GitLab repository: (https://pscode.lioncloud.net/sde-onboarding-engagement/vinoth.kumar/cashdrawerdeviceapi)
2. Open the project in your preferred text editor (e.g., IntelliJ IDEA).
3. Ensure that Java 11 is selected as the JDK for the project.
4. Build the project using Gradle. Run the following command in the project root directory:
   ```
   ./gradlew build
   ```
5. Once the build process completes successfully, you can proceed to the next section to use the API.

## Usage

To use the Cash Drawer Device API, follow these steps:

1. Start the API server by executing the following command:
2. The API server will start running on your local machine's specified port (default: 8080).
3. Access the Swagger UI documentation by opening the following URL in your web browser:( http://localhost:8080/swagger-ui/)

The Swagger UI provides an interactive interface to explore and test the API endpoints.

## Endpoints

The Cash Drawer Device API exposes the following endpoints:

1. `POST /v1/cashdrawer/{deviceId}` - Initializes the CashDrawer.
2. `PUT /v1/cashdrawer/{deviceId}/open-drawer` - Opens the CashDrawer Device.
3. `DELETE /v1/cashdrawer/{deviceId}` - Closes the CashDrawer Device.
4. `GET /v1/cashdrawer/{deviceId}` - Gets the status of the CashDrawer Device.

You can view the endpoints and use them in the Swagger UI.

## Testing

The Cash Drawer Device API is thoroughly tested using JUnit 5 and Mockito. To run the tests, execute the following command:
```
./gradlew test
```
The test suite includes unit tests and end-to-end BDD tests for the device service simulator.

## Monitoring and Tracing

Open Telemetry is integrated into the API for tracing, monitoring, and logging purposes. The API emits telemetry data that can be visualized using Grafana and Prometheus.

To view the telemetry metrics, follow these steps:

1. Install and set up Grafana and Prometheus on your local machine or a server.
2. Configure the Cash Drawer Device API to send telemetry data to the Prometheus endpoint.
3. Access the Grafana dashboard to view the real-time performance metrics.

Tracing details can be monitored using the Jaeger UI.

To view the tracing details, follow these steps:

1. Start Docker and run the following commands to start the Jaeger UI:
```
docker build -t cashdrawerdeviceapi .
docker run -p 8081:8081 cashdrawerdeviceapi
```
2. Go to the Jaeger UI at its default port: (http://localhost:16686/)
3. Run the application and execute various endpoints through Postman.
4. Refresh the Jaeger UI page, and "cash-drawer-device-api" will be added as one of the services, allowing you to view its traces.




