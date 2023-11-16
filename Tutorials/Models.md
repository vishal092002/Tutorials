# Models : 

The term "models" in the context of coding, particularly in database management and web development, typically refers to representations of tables in a database. They are often used in object-relational mapping (ORM), where these models act as the bridge between the object-oriented code and the relational database. Models define the structure of the data, the relationships between different parts of the data, and the way the data can be queried and manipulated.


```bash
model User {
  id        Int      @id @default(autoincrement())
  email     String   @unique
  name      String?
  pets      Pets[]
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Pets {
  id        Int      @id @default(autoincrement())
  type      String
  name      String?
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  User      User?    @relation(fields: [userId], references: [id])
  userId    Int?
}
```
### Model: User
- **id**: An integer that uniquely identifies each user. It is the primary key (`@id`) and automatically increments (`@default(autoincrement())`).
- **email**: A string representing the user's email. It is marked as unique (`@unique`), meaning no two users can have the same email.
- **name**: An optional (`?`) string for the user's name. The `?` signifies that this field can be null, indicating that it's not mandatory to have a name.
- **pets**: This indicates a one-to-many relationship with the `Pets` model. A user can have multiple pets, hence `Pets[]`.
- **createdAt**: A date-time field that is automatically set to the current date and time when a new user is created (`@default(now())`).
- **updatedAt**: A date-time field that automatically updates every time the user's data is modified (`@updatedAt`).

### Model: Pets
- **id**: Similar to the User model, it's an integer that acts as a primary key and automatically increments.
- **type**: A string that presumably represents the type of pet.
- **name**: An optional string for the pet's name.
- **createdAt** and **updatedAt**: These fields function identically to those in the User model, representing the creation and last update time of the pet record.
- **User**: This establishes a many-to-one relationship with the `User` model. Each pet is associated with a user (`User?` indicates that the pet may or may not be associated with a user).
- **userId**: An optional integer field (`Int?`) that stores the user's ID. This field is used to establish the relationship with the `User` model, as indicated by `@relation(fields: [userId], references: [id])`.
