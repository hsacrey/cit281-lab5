## Lab 5

In this lab, I used Postman to create a CIT 281 collection and folders.

I also created a Node.js Fastify server application with GET and post handling that responds with JSON.

I used Postman to test server GET routes and to send a POST.

I had an array of Student objects the following route options: view all students, view a specific student by id, and an unmatched route.

#### All students:
![all students](AllStudents.png)

#### Specific Student:
![one student](SingleStudent.png)

#### Posting a Student:
![student post](StudentPost.png)

#### Unmatched route:
![unmatched](Unmatched.png)

#### fastify-server.js:
```javascript
/*
    CIT 281 Lab 4
    Name: Hunter Sacrey
*/
//REQUIRE STATMENTS:
const fastify = require("fastify")();

const students = [
  {
    id: 1,
    last: "Last1",
    first: "First1",
  },
  {
    id: 2,
    last: "Last2",
    first: "First2",
  },
  {
    id: 3,
    last: "Last3",
    first: "First3",
  }
];
  fastify.get("/cit/student", (request, reply) => {

    reply
      .code(200)
      .header("Content-Type", "text/html; charset=utf-8")
      .send(JSON.stringify(students));
});
  fastify.get("/cit/student/:id", (request, reply) => {
    const id = request.params.id;
    reply
      .code(200)
      .header("Content-Type", "text/html; charset=utf-8")
      .send(JSON.stringify(students[parseInt(id)-1]));
});
fastify.get("*", (request, reply) => {
  reply
  .code(404)
  .header("Content-Type", "application.html; charset=utf-8")
  .send("<h1>PATH NOT FOUND</h1>");
});

//POST
fastify.post("/cit/student", (request, reply) => {
  const {first,last} = request.body;
  students.push({ id: students.length+1, last: last, first: first});
  let response = students[students.length-1];
  reply
    .code(200)
    .header("Content-Type", "application/json; charset=utf-8")
    .send(response);
});

// Start server and listen to requests using Fastify
const listenIP = "localhost";
const listenPort = 8080;
fastify.listen(listenPort, listenIP, (err, address) => {
  if (err) {
    console.log(err);
    process.exit(1);
  }
  console.log(`Server listening on ${address}`);
});
```
