# Note Management Microservice

## Requirements Architecture
1. The microservice should be developed using Spring Boot.
2. It should have an entity called "Note" with the following attributes:
   - Title
   - Text body
   - Categories (many-to-many relationship)
3. The microservice should provide CRUD (Create, Read, Update, Delete) endpoints and services for the "Note" entity.
4. The database should be an H2 database.
5. Whenever an operation (create, update, delete) is performed on a "Note" entity, a Kafka event should be sent to notify that the operation has happened.
6. A consumer of the Kafka event should log the received event.
7. The microservice should follow a layered architecture, separating the presentation, service, and data access layers.

## Use Cases in Gherkin Format

```gherkin
Feature: Note Management

  Scenario: Create a new note
    Given I have a valid note with a title, text body, and categories
    When I send a POST request to the "/notes" endpoint with the note details
    Then the note should be created successfully
    And a Kafka event should be sent indicating the creation of the note
    And the consumer should log the received event

  Scenario: Retrieve a note by ID
    Given a note with ID "123" exists in the system
    When I send a GET request to the "/notes/123" endpoint
    Then the note with ID "123" should be returned
    And the response should contain the title, text body, and categories of the note

  Scenario: Update an existing note
    Given a note with ID "123" exists in the system
    When I send a PUT request to the "/notes/123" endpoint with updated note details
    Then the note with ID "123" should be updated successfully
    And a Kafka event should be sent indicating the update of the note
    And the consumer should log the received event

  Scenario: Delete a note
    Given a note with ID "123" exists in the system
    When I send a DELETE request to the "/notes/123" endpoint
    Then the note with ID "123" should be deleted successfully
    And a Kafka event should be sent indicating the deletion of the note
    And the consumer should log the received event

  Scenario: Retrieve all notes
    Given there are multiple notes in the system
    When I send a GET request to the "/notes" endpoint
    Then all the notes should be returned
    And each note should contain the title, text body, and categories
```

## Layered Architecture

The microservice should follow a layered architecture consisting of the following layers:

- **Presentation Layer**: Contains the REST controllers that handle the incoming HTTP requests and return the appropriate responses.
- **Service Layer**: Contains the business logic and coordinates the interaction between the presentation layer and the data access layer.
- **Data Access Layer**: Handles the communication with the H2 database and performs the database operations.

The Kafka producer should be integrated into the service layer to send events whenever a note is created, updated, or deleted. The Kafka consumer should be a separate component that listens to the events and logs them accordingly.

## Additional Notes

- The microservice should be implemented using Spring boot, leveraging its features and dependencies for building RESTful APIs and handling database operations.
- The H2 database should be used as an in-memory database for simplicity and ease of setup. However, the microservice architecture should allow for easy switching to a different database if needed.
- The Kafka integration should be done using Kafka, which provides a simple and convenient way to send and consume Kafka events in a Spring Boot application.
- Proper error handling and validation should be implemented to ensure the integrity and reliability of the microservice.
- Unit tests and integration tests should be written to verify the functionality of the microservice and ensure its correctness.

