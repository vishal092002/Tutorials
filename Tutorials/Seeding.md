# Seeding Tutorial
Database seeing is the process of populating a datbase with an inital set of data. In our project we are using Prisma and Faker to do this. 

## Prisma ###
Prisma is an ORM or Object relational manager that is used  to make connections to a datbase easy with auto generated queries that is tailored to the database schema that you create.  

## Faker ##
Faker is a library that is used to generate fake but realistic data. It is commanly used in software development for testing, databases and creating mock data for applications. 

## Prisma Schema ##
```bash
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "sqlite"
  url      = "file:./dev.db"
}

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
The prisma schema that we have above defines two modesl users and pets. Each user can have multipal diffrent pets which is indicated by the parts field in the user model. The Pets model has a feild user that creates a relationshoip between the User model and Pet model which indicates ownership. 

## Commands needed to seed database##
```bash
npm install 
```
This command installs the project depndacies defined in the package.json file 

```bash
npm run dev 
```
This command runs the developmetnserver. This is so that we can see the records after the datbase is seeded. 

```bash
npx prisma migrate dev
```
This command is used to update the version controle of the schema. 

```bash
npx prisma db seed
```
This command is used to exacute the seeding scriopt. THis command uses prisma to pipulate the datbase with the data that is defind in your seed script. 

```bash
npm run test 
```
This runs the tests for our application that we have defind. 


now when we go to where our servuer is running which is on http://localhost:3000/prisma
we should see the following which uis the records that have been created through seeding. 

![Alt text](image.png)