
```markdown
# Social Media RESTful API

## Features
- **User Management**: CRUD operations for user profiles
- **Post System**: Create, read, update, and delete posts
- **Social Features**: Comments, likes, and followers
- **Content Negotiation**: Supports JSON and XML responses
- **Pagination**: Efficient data retrieval
- **API Documentation**: Interactive docs page

## Installation

```bash
# Clone repository
git clone https://github.com/your-repo/social-media-api.git
cd social-media-api

# Install dependencies
npm install

# Configure environment
cp .env.example .env

# Start development server
npm run dev
```

## API Endpoints

### Users
| Method | Endpoint         | Description                 |
|--------|------------------|-----------------------------|
| GET    | /api/users       | List all users (paginated)  |
| POST   | /api/users       | Create new user             |
| GET    | /api/users/:id   | Get specific user           |
| PUT    | /api/users/:id   | Update user                 |
| DELETE | /api/users/:id   | Delete user                 |

### Posts
| Method | Endpoint         | Description                 |
|--------|------------------|-----------------------------|
| GET    | /api/posts       | List all posts              |
| POST   | /api/posts       | Create new post             |
| GET    | /api/posts/:id   | Get specific post           |
| PUT    | /api/posts/:id   | Update post                 |
| DELETE | /api/posts/:id   | Delete post                 |


## Request/Response Examples

**Create User:**
```bash
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{
    "username": "new_user",
    "email": "user@example.com",
    "password": "securepassword"
  }'
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "4",
    "username": "new_user",
    "email": "user@example.com",
    "created_at": "2023-05-15"
  }
}
```

## Testing
```bash
# Run automated tests
npm test

# Test with different content types
curl -H "Accept: application/xml" http://localhost:3000/api/users
```


---

