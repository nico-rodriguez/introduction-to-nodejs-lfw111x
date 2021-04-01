# Service Mocking

## Requirements

* `node 14.x`
* `fastify ^3.0.0`
* `fastify-cors ^5.2.0`

## Description

There are two applications: the mock server in `mock-server` and a client in `client/`.

## Running the applications

The mock server is implemented with `fastify`, so run `npm run dev` inside `mock-server`; it listens on `127.0.0.1:3000`. For the client, use whatever deploy method you like (for instance with `serve` package: `npx serve client/static`) and go to `localhost:5000`.

## Initial data

Initially, the server holds the following data:

``` js
[
  // electronics
  {id: 'A1', name: 'Vacuum Cleaner', rrp: '99.99', info: 'The suckiest vacuum in the world.'},
  {id: 'A2', name: 'Leaf Blower', rrp: '303.33', info: 'This product will blow your socks off.'},
  // confectionery
  {id: 'B1', name: 'Chocolate Bar', rrp: '22.40', info: 'Delicious overpriced chocolate.'}
]
```

## Routes and Methods

The server supports `GET` and `POST` methods at routes `/confectionery` and `/electronics`.

The first method returns a list of objects of the selected type; the later 'inserts' a new object in the server. Before the insertion, the index of the new object is computed as the value of the last index plus one. This is done by `calculateId` in `plugins/data-utils.js`. Then, the new object is inserted in the `data` array of `routes/confectionery/index.js` using the `request.mockDataInsert` method defined through `fastify.decorate` in `plugins/data-utils.js` (this makes the method available for all routes).

This makes the server statefull, which is against the recommended use of `NodeJS`. However, this is fine for a mock server.

### Data validation

There's no data validation in the `POST` method, since it's a mock service (the only reason to add validation would be to mock the flow of validation interactions between server and client).

## CORS policy

Since the client and the mock server run on different ports, there's a problem with CORS. That's why `fastify-cors` sets `Access-Control-Allow-Origin` header for every route defined.
