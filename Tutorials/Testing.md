# Testing Tutorial

### Introduction
Playwright Test provides a robust and versatile platform for automating browser-based tests, covering a wide range of use cases from simple page navigations to complex user interactions and validations. It is particularly useful for developers and testers aiming to ensure high-quality web applications across various browsers and environments.

### Writing Tests
- **Using Playwright API**: Tests are written using the Playwright API, which provides a high-level way to control browser behavior.
- **Interaction Steps**: These tests describe the steps to interact with the web application, like navigating to a URL, clicking elements, and checking expected outcomes.

### Running Tests
- **Browser Instances**: When you run a Playwright test, it launches a browser instance either with or without the UI.
- **Execution**: It then executes the actions described in your test scripts on the required browser instance.
- **Supported Browsers**: These browsers include Google Chrome and Microsoft Edge (with Chromium), Apple Safari (with WebKit), and Mozilla Firefox.
- **How to Run**: You can run Playwright tests using the `npm run test` command. The tests are executed in github actions as well.
### Our Code
The following is the code that we have written for our program:
 
```bash
// @ts-check
const prisma = require('../src/lib/prismaClient')
const { addFakeUserToDatabase, generateFakeUser } = require('../src/lib/seed');
const { test, expect } = require('@playwright/test');


test.beforeEach(async ()=>{ //clear the database before each test and add 1 user
  await prisma.user.deleteMany({})
  await prisma.pets.deleteMany({})
  await addFakeUserToDatabase({amount: 1}).then(()=>{
    prisma.$disconnect()
})
})

test('GET on User', async ({ request }) => {
  const req = await request.get(`/api/user`)
  expect(req.status()).toBe(200)
  expect(await req.json()).not.toEqual([])
});
test('GET on Pets', async ({ request }) => {
  const req = await request.get(`/api/pets`)
  expect(req.status()).toBe(200)
  expect(await req.json()).not.toEqual([])
});
test('POST on Pets', async ({ request }) => {
  const req = await request.post(`/api/pets`,{
    data: {"type":"rabbit","name":"Douglas"}
  })
  let jsonvalue = await req.json()
  expect(req.status()).toBe(201)
  expect(jsonvalue).not.toBe([])
}
);
test('POST on Users', async ({ request }) => {
  const req = await request.post(`/api/user`,{
    data: {"email":"test@test.com","name":"Douglas"}
  })
  let jsonvalue = await req.json()
  expect(req.status()).toBe(201)
  expect(jsonvalue).not.toBe([])
  expect(jsonvalue.name).toBe("Douglas")
  expect(jsonvalue.email).toBe("test@test.com")
});

test('GET by ID on User', async ({ request }) => {
  let firstUser = await prisma.user.findFirst()
  let userID = firstUser.id
  const req = await request.get(`/api/user/${userID}`)
  expect(req.status()).toBe(200)
  expect(await req.json()).not.toEqual([])
  let jsonvalue = await req.json()
  expect(jsonvalue.name).toBe(firstUser.name)
  expect(jsonvalue.email).toBe(firstUser.email)
});
test('GET by ID on Pets', async ({ request }) => {
  let firstPet = await prisma.pets.findFirst()
  let petID = firstPet.id
  const req = await request.get(`/api/pets/${petID}`)
  expect(req.status()).toBe(200)
  expect(await req.json()).not.toEqual([])
  let jsonvalue = await req.json()
  expect(jsonvalue.name).toBe(firstPet.name)
  expect(jsonvalue.type).toBe(firstPet.type)
});
test("PATCH on User email", async ({ request }) => {

  let firstUser = await prisma.user.findFirst()
  let userID = firstUser.id
  const req = await request.patch(`/api/user/${userID}`,{
    data: {"email":"test2@test.com"}
  })
  expect(req.status()).toBe(200)
  expect(await req.json()).not.toEqual([])
  let jsonvalue = await req.json()
  expect(jsonvalue.email).toBe("test2@test.com")
});
test("PATCH on User name", async ({ request }) => {
  
  let firstUser = await prisma.user.findFirst()
  let userID = firstUser.id
  const req = await request.patch(`/api/user/${userID}`,{
    data: {"name":"test2"}
  })
  expect(req.status()).toBe(200)
  expect(await req.json()).not.toEqual([])
  let jsonvalue = await req.json()
  expect(jsonvalue.name).toBe("test2")
});

test("PATCH on Pets name", async ({ request }) => {
    
    let firstPet = await prisma.pets.findFirst()
    let petID = firstPet.id
    const req = await request.patch(`/api/pets/${petID}`,{
      data: {"name":"test"}
    })
    expect(req.status()).toBe(200)
    expect(await req.json()).not.toEqual([])
    let jsonvalue = await req.json()
    expect(jsonvalue.name).toBe("test")
  });

  test("PATCH on Pets type", async ({ request }) => {
      
      let firstPet = await prisma.pets.findFirst()
      let petID = firstPet.id
      const req = await request.patch(`/api/pets/${petID}`,{
        data: {"type":"test"}
      })
      expect(req.status()).toBe(200)
      expect(await req.json()).not.toEqual([])
      let jsonvalue = await req.json()
      expect(jsonvalue.type).toBe("test")
    });

test('DELETE on User', async ({ request }) => {
  let firstUser = await prisma.user.findFirst()
  let userID = firstUser.id
  const req = await request.delete(`/api/user/${userID}`)
  expect(req.status()).toBe(204)
  expect(await req.text()).toBe("")
});

test('DELETE on Pets', async ({ request }) => {
  let firstPet = await prisma.pets.findFirst()
  let petID = firstPet.id
  const req = await request.delete(`/api/pets/${petID}`)
  expect(req.status()).toBe(204)
  expect(await req.text()).toBe("")
});

test('GET on Deleted User', async ({ request }) => {
  let firstUser = await prisma.user.findFirst()
  let userID = firstUser.id
  const req1 = await request.delete(`/api/user/${userID}`)
  const req2 = await request.get(`/api/user/${userID}`)
  expect(req2.status()).toBe(404)
  expect(await req2.text()).toBe("No user with ID found")
});
test('GET on Deleted Pets', async ({ request }) => {
let firstPet = await prisma.pets.findFirst()
let petID = firstPet.id
const req1 = await request.delete(`/api/pets/${petID}`)
const req2 = await request.get(`/api/pets/${petID}`)
expect(req2.status()).toBe(404)
expect(await req2.text()).toBe("No pet with ID found")
})
```
### Setup Depenencies: ###
The Script starts of by importing the nesseasry modeuls. 

'prisma' is used to interact with the database

 addFakeUserToDatabase  and generateFakeUser for seeding test data, 
 
 and { test, expect }from@playwright/test for writing and running tests.

 ### Before Each Test: ###
 The beforeEach hook is set up to run before each test. It clears the user and pets tables in the database and adds a single fake user for a consistent testing environment.

## API Tests:

### GET Tests
- **Description**: These tests check if the GET requests to the user and pets API return a status code of `200 (OK)` and the response is not empty.

### POST Tests
- **Description**: These tests verify if POST requests for adding new users and pets work correctly by checking the response status is `201 (Created)` and the response body is as expected.

### GET by ID Tests
- **Description**: These tests fetch a user and a pet by their ID and check if the correct data is returned.

### PATCH Tests
- **Description**: These tests update (PATCH) the details of a user and a pet, such as email, name, and pet type, and verify if the updates are successful.

### DELETE Tests
- **Description**: These tests remove a user and a pet from the database and ensure the response status is `204 (No Content)`.

### GET on Deleted Data
- **Description**: These tests attempt to fetch a deleted user and pet by their ID, expecting a `404 (Not Found)` status and a specific message indicating the absence of the user or pet.

### Test Assertions
- **Description**: Each test includes assertions using `expect` to validate the response status codes, response body contents, and to ensure that the data is as expected.
