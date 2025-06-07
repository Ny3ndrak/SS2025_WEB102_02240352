# Secure Authentication System Implementation 

## Project Overview

This project demonstrates how to build a robust authentication system using modern web technologies. The implementation focuses on creating a secure REST API with email/password authentication, leveraging TypeScript, Hono, PostgreSQL, and Prisma with JWT tokens.

## Core Features

### Authentication System Components
- **User Registration**: Email-based account creation with validation
- **Secure Login**: Password authentication with JWT token generation
- **Protected Endpoints**: Route protection through middleware
- **Account Management**: Integrated balance tracking system

### Security Measures
- **Password Protection**: Bcrypt hashing with salt
- **Token-Based Auth**: JWT implementation with expiration
- **Error Handling**: Secure failure responses
- **Database Security**: Proper ORM integration

## Technical Requirements

### Development Environment
- **Runtime**: Bun v1.x or newer
- **Database**: PostgreSQL
- **IDE**: VS Code (recommended with Prisma extension)

## Setup Instructions

1. **Clone and Initialize**
```bash
git clone https://github.com/rubcstswe/web102-hono-auth-jwt-prisma-forked.git
cd web102-hono-auth-jwt-prisma-forked
bun install
```

2. **Database Configuration**
```bash
bunx prisma db push
bunx prisma generate
```

3. **Launch Development Server**
```bash
bun run dev
```

## Database Architecture

### User Model
```prisma
model User {
  id           String    @id @default(uuid())
  email        String    @unique
  hashPassword String
  Account      Account[]
}
```

### Account Model
```prisma
model Account {
  id      String @id @default(uuid())
  userId  String
  user    User   @relation(fields: [userId], references: [id])
  balance Int    @default(0)
}
```

## API Endpoints

### Authentication
| Method | Endpoint   | Description               | Auth Required |
|--------|------------|---------------------------|---------------|
| POST   | /register  | Create new user account   | No            |
| POST   | /login     | Authenticate user         | No            |

### Protected Resources
| Method | Endpoint                     | Description               | Auth Required |
|--------|------------------------------|---------------------------|---------------|
| GET    | /protected/account/balance   | Get account balance       | Yes           |

## Implementation Details

### User Registration
```typescript
app.post("/register", async (c) => {
  try {
    const { email, password } = await c.req.json();
    
    const hashedPassword = await Bun.password.hash(password, {
      algorithm: "bcrypt",
      cost: 4,
    });

    const user = await prisma.user.create({
      data: {
        email,
        hashedPassword,
        Account: { create: { balance: 0 } }
      }
    });

    return c.json({ message: `${user.email} created successfully` });
  } catch (error) {
    // Handle duplicate email error
    if (error.code === 'P2002') {
      return c.json({ message: 'Email already exists' });
    }
    throw error;
  }
});
```

### User Login
```typescript
app.post("/login", async (c) => {
  const { email, password } = await c.req.json();
  
  const user = await prisma.user.findUnique({
    where: { email },
    select: { id: true, hashedPassword: true }
  });

  if (!user) return c.json({ message: "User not found" });

  const passwordMatch = await Bun.password.verify(
    password,
    user.hashedPassword,
    "bcrypt"
  );

  if (passwordMatch) {
    const token = await sign(
      { sub: user.id, exp: Math.floor(Date.now() / 1000) + 3600 },
      "mySecretKey"
    );
    return c.json({ token });
  }
  
  throw new HTTPException(401, { message: "Invalid credentials" });
});
```

### Protected Route Example
```typescript
app.use("/protected/*", jwt({ secret: 'mySecretKey' }));

app.get("/protected/account/balance", async (c) => {
  const payload = c.get('jwtPayload');
  const account = await prisma.account.findFirst({
    where: { userId: payload.sub }
  });
  return c.json({ balance: account?.balance });
});
```

## Security Best Practices

### Password Security
- Always hash passwords using bcrypt
- Use appropriate cost factor (higher for production)
- Never store plain text passwords

### JWT Implementation
- Store secret keys in environment variables
- Set reasonable token expiration
- Keep payload minimal
- Always use HTTPS in production

## Testing Procedures

### Registration Test
```bash
curl -X POST http://localhost:3000/register \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"SecurePass123"}'
```

### Login Test
```bash
curl -X POST http://localhost:3000/login \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"SecurePass123"}'
```

### Protected Endpoint Test
```bash
curl -X GET http://localhost:3000/protected/account/balance \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

## Key Technical Concepts

1. **Authentication Flow**: Clear separation of registration and login processes
2. **Password Hashing**: Secure storage using bcrypt algorithm
3. **Token-Based Auth**: Stateless JWT implementation
4. **Middleware Protection**: Route guarding architecture
5. **Error Handling**: Secure and consistent error responses

## Production Considerations

- **Environment Variables**: For all sensitive configuration
- **HTTPS Enforcement**: Essential for production deployments
- **Rate Limiting**: Protection against brute force attacks
- **Input Validation**: For all user-provided data
- **Monitoring**: Comprehensive logging system

This implementation provides a solid foundation for building secure authentication systems in modern web applications.