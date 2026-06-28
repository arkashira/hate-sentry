 # Tech-Spec.md

## Stack
- Language: Python 3.9 (for its extensive libraries and community support)
- Framework: Flask (for building APIs quickly and easily)
- Runtime: Uvicorn (for running the Flask app as a standalone server)

## Hosting
- Free-tier-first: Heroku (for easy deployment and scaling)
- Specific platforms: AWS Elastic Beanstalk (for production deployment)

## Data Model
### Tables/Collections
- Users (id, username, password_hash, email)
- Posts (id, user_id, content, timestamp, is_moderated)
- Moderation_Log (id, post_id, moderator_id, moderation_status, moderation_reason, timestamp)

### Key Fields
- `id`: unique identifier for each record
- `username`: unique username for each user
- `password_hash`: hashed password for each user
- `email`: email address for each user
- `content`: the text content of each post
- `timestamp`: the timestamp when each post was created
- `is_moderated`: boolean indicating whether the post has been moderated or not
- `moderation_status`: the moderation status of the post (e.g., approved, rejected, needs_review)
- `moderation_reason`: the reason for the moderation status (if applicable)
- `moderator_id`: the ID of the moderator who moderated the post

## API Surface
### Endpoints
1. `POST /api/v1/register`: Register a new user
2. `POST /api/v1/login`: Login a user
3. `POST /api/v1/posts`: Create a new post
4. `GET /api/v1/posts`: Retrieve a list of all posts
5. `GET /api/v1/posts/{post_id}`: Retrieve a specific post by ID
6. `PUT /api/v1/posts/{post_id}/moderate`: Moderate a specific post by ID
7. `GET /api/v1/posts/moderated`: Retrieve a list of all moderated posts
8. `GET /api/v1/posts/unmoderated`: Retrieve a list of all unmoderated posts

### Method/Path/Purpose
1. POST: Register a new user (creates a new user in the Users table)
2. POST: Login a user (verifies the user's credentials and returns a JWT)
3. POST: Create a new post (creates a new post in the Posts table)
4. GET: Retrieve a list of all posts (retrieves all posts from the Posts table)
5. GET: Retrieve a specific post by ID (retrieves a specific post from the Posts table by ID)
6. PUT: Moderate a specific post by ID (updates the is_moderated field and Moderation_Log table for the specified post)
7. GET: Retrieve a list of all moderated posts (retrieves all posts from the Posts table where is_moderated is true)
8. GET: Retrieve a list of all unmoderated posts (retrieves all posts from the Posts table where is_moderated is false)

## Security Model
- Auth: JWT authentication for user management
- Secrets: Environment variables for storing sensitive information (e.g., database credentials)
- IAM: AWS IAM roles for production deployment

## Observability
- Logs: Logging using Python's built-in logging module
- Metrics: Metrics using Prometheus and Grafana for monitoring application performance
- Traces: Distributed tracing using Jaeger for debugging and understanding the flow of requests through the system

## Build/CI
- Build: Use `pip` to install dependencies and build the application
- CI: Use GitHub Actions for continuous integration and deployment