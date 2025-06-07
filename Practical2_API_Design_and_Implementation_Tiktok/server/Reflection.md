# Practical 2: TikTok API Implementation Reflection  

### Applied Concepts  

#### 1. **RESTful API Design**  
- Implemented resource-based endpoints:  
  ```plaintext
  /api/videos
  /api/users/:id/followers
  /api/comments/:id/likes
  ```
- Proper HTTP method usage:  
  - `GET` for retrieval  
  - `POST` for creation  
  - `PUT` for updates  
  - `DELETE` for removal  

#### 2. **Express.js Architecture**  
| Layer | Responsibility | Example File |
|-------|----------------|--------------|
| Routes | Endpoint definitions | `routes/videos.js` |  
| Controllers | Business logic | `controllers/userController.js` |  
| Middleware | Request processing | `middleware/auth.js` |  

#### 3. **In-Memory Data Modeling**  
Created temporary data structures:  
```javascript
// In models/index.js
let videos = [
  {
    id: 1,
    title: "First video",
    likes: 0,
    comments: []
  }
]
```

#### 4. **API Testing**  
- Verified all endpoints with:  
  - `cURL` commands  
  - Postman collection    

---

## 💭 Reflection 

### Key Learnings  

1. **REST Principles in Practice**  
- Learned the importance of:  
  - Consistent endpoint naming (`/resources/:id/sub-resources`)  
  - Proper status codes (200, 201, 404)  
  - Idempotent operations (especially for PUT/DELETE)  

2. **Middleware Value**  
Discovered that:  
- `morgan` logging reduced debugging time by ~30%  
- `cors` middleware was essential for frontend integration  
- Custom middleware simplified auth validation  

3. **Project Organization**  
The MVC separation:  
- Made collaboration easier  
- Allowed focused testing  
- Simplified future database integration  

### Challenges & Solutions  

| Challenge | Solution | Outcome |
|-----------|----------|---------|
| Endpoint conflicts | Used Router nesting | Cleaner route definitions |
| Data persistence | Implemented ID generation | Reliable resource referencing |
| Error handling | Created uniform error format | Better client-side processing |
| Rate limiting | Added `express-rate-limit` | Prevented API abuse |

**Example Code Improvement**:  
```javascript
// Before
app.get('/videos', (req, res) => {...})

// After
const router = express.Router()
router.route('/')
  .get(videoController.getAllVideos)
  .post(videoController.createVideo)
```

---
