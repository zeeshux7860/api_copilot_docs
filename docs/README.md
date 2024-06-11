

# Without State Management

API Copilot provides flexibility for developers who choose not to use state management. Here's how it works without state management:

#### Folder Structure

1. **Model**: Contains data models representing different entities fetched from the API. These models define the structure of the data received from the API.

2. **Query**: Houses functions responsible for making HTTP requests to the API endpoints. These functions retrieve data from the server and handle responses.

3. **Util**: Contains utility functions used across the application, such as handling HTTP requests, file operations, and global configurations.

#### HTTP Requests and Global Handler

- **Request Functions**: Functions in the `query` folder define HTTP requests to the API endpoints. They utilize the `http` package to make GET, POST, PUT, or DELETE requests.

- **Global Handler**: The `GlobalHandler` class provides common configurations for HTTP requests, such as headers and authentication tokens. It ensures consistent behavior across requests.


#### Example Workflow

1. **Data Model Definition**: Define data models in the `model` folder to represent the structure of data received from API endpoints.

2. **Query Configuration**: Define functions in the `query` folder to make HTTP requests to API endpoints, handling parameters and response parsing.

3. **Usage in Widgets**: Use query functions directly in widgets to fetch data from APIs and update UI components based on the retrieved data.

API Copilot provides a lightweight approach to integrating APIs into Flutter applications for developers who prefer not to use state management.







---

## Conclusion

API Copilot streamlines the process of API integration, empowering developers to build web applications more efficiently. With its intuitive interface and powerful features, API Copilot is your go-to tool for seamless API integration.

---

This documentation serves as a guide to help you navigate and utilize API Copilot effectively. If you have any questions or encounter issues while using the tool, don't hesitate to reach out to our support team.
