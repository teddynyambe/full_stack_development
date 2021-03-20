# 1. Middleware design and best practices
The middle design is brocken down into **controllers** (exposing the API), **services** (business logic to perform a specific task - these are exposed to the outside world by controllers, **repositories** these provide abstraction of the database (**model**).

## 1.1 Design thought
Think its best to start from the data (model) and group your way up to the controllers.

## 1.2 Data Model
### Database connection
During design start with in-memory database during testing introduce real db like mysql.
  
