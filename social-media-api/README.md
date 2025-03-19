## Practical 1: API Design and Implementation

# Overview
Designed and implemented a RESTful API for a social media platform similar to Instagram, handling users, posts, comments, likes, and followers. The API follows REST principles, supports pagination, error handling, and content negotiation.

# Objectives

-Design RESTful API endpoints with meaningful URIs.
-Implement CRUD operations for key resources.
-Handle errors gracefully.
-Add pagination for large datasets.
-Support content negotiation (JSON/XML).
-Document the API with an HTML page.
-Implementation Steps

1. Project Setup
-Initialized a Node.js project with express, morgan, cors, and helmet.
-Structured folders for controllers, routes, middleware, and utilities.
-Configured environment variables.

2. API Design
Endpoints for users, posts, comments, likes, and followers.
 Example:
-GET /users - List users
-POST /users - Create a user
-PUT /users/{id} - Update user

3. Development
-Implemented controllers for CRUD operations.
-Defined routes to map HTTP methods.
-Added middleware for error handling, logging, and content negotiation.
-Used mock data instead of a real database.

4. Additional Features
-Pagination: Query parameters (page, limit).
-Error Handling: Custom error responses.
-Content Negotiation: Supports JSON and XML.

5. Documentation
Created an HTML page (public/docs.html) with endpoint descriptions and examples.
