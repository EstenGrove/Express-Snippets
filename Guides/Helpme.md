# Guide for Structure & Design Concepts in Express/NodeJS
This document contains various descriptions, definitions and high-level concepts.


---

## Routing & Routers

- When setting up a router it's best to separate each route handler (ie '/todos') in a separate file:
  - This way each route API & it's code can be amended without effecting the other routes and their code.
- `router.get('/', someRouteModule);`: merely defines what code should run when a router receives a request at a specific route
- `app.use('/todos', todoModule);`: defines that the 'someModule' also named 'todoModule' is to be called when a 'get' request hits '/todos'.








