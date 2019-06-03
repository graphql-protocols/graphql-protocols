# GraphQL Contracts

> WARNING: We're working on this 💪. For discussion purposes only at this moment.

The simplest way to describe services for server-to-server and server-to-client communication via GraphQL interfaces.

## What is this?

Basically, a repository of GraphQL interfaces. Think 'services', WSDL, CORBA that defines how servers or servers-to-client can communicate to eachother via a common GraphQL interface. The core idea is that a server can simply request what contracts (interfaces) a GraphQL server implements and can start communicating with that server if it supports that queried contract.

## What does this solve?

It solves the rules of engagement and lack of standardisation on how server-to-server or clients should talk to eachother and therefore enchancing interopability for the open web. It makes data and operations on it _interchangable_.

For instance: There are 20 domain name registrars where you can buy domain names. Automating this would require implementing 20 specific APIs. Or you could make them agree on one simple interface. Ofcourse getting them to implement this API is a whole different ballgame, but making sure the effort is minimal would improve your chances of getting this done.

With the help of GraphQL contracts one could define a single interface that all registars would need to implement. This way you could build a registration client allowing to register at those vendors easily. That's the aim of GraphQL contracts. Making it easy to build interfaces for distributed services.

The same goes for an _notifcation_ service. You define the contract and the resulting notification implementation can differ from slack to email. As long as you adhere to the GraphQL SDL contract, the resulting consumers wont break either.

## Why GraphQL for service descriptions

- Simple introspection of what interfaces/contracts an endpoint implements by just viewing the query or mutation type.
- Composable. Simple to compose and extend.
- A dead simple principle, if you know GraphQL you know it already ❤️.

And ofcourse the benefits of GraphQL itself.

- Self documenting
- Strongly typed
- Supports deprecation and versioning
- The whole GraphQL ecosystem such as client description.

## Current draft contracts

All of the contracts are currently in DRAFT state and used as input for discussion.

- [Request Tokens](draft/token-request.md) - A self service way of retrieving an API token from a service for communication. With this contract you can identify yourself to the service and start using other contracts (APIs) on that service if they require authentication.
- [Postbox](draft/postbox.md) - Send messages to a service / site

## How does it work technically?

The contracts are just _Plain Old GraphQL Interfaces_ written in [SDL](https://graphql.org/learn/schema/). Servers and clients need to implement these interfaces and types in order to speak to eachother. You can see GraphQL contracts more as a _best practice pattern_ than an actual technical solution.

Usually such a contract exists out of a couple of parts defined in GraphQL schema definition. Lets say this trivial example where you have a bunch of servers that send messages to eachother. By connecting to a server that supports this contract you can easily send a message to the owner of that contract.

The types you need for your service (You will share this file with your implementers):

```graphql

# The type you need for your service
type Message {
  message: String
}

# The query interface
interface Postbox {
  messages: [Message]
}

# The mutation interface
interface PostboxMutatations {
  postMessage(message: String): Message
}
```

Then you need to hook them up to your query type.

Add to your query type:

```graphql
type Query implements Postbox
```

Add to your mutation types:

```graphql
type Mutation implements PostboxMutatations
```

And thats it. The idea is that the contract part is _reusable_. And that other implementers only need to implement your interface SDL in order for clients to talk to.

## Contract writing best practices

This will help you building contracts that are easy to maintain.

### Keep contracts small

Keep your contracts as easy and small as possible. The changes that complicated contracts will get implemented are quite small. The easier to implement, the higher changes of adoption.

### Write it for humans

- Make it conversational in terms of naming for mutations and fields
- Make it easy to implement with just GraphiQL. If its painful to use the contract in GraphiQL it will be hard to implement too.
- Keep it intuitive in terms of naming and flow
- Communicate as if you're communicating to a human

### Adhere to GraphQL conventions

- Things under `viewer` should be tied to the `viewer` (i.e current authenticated user)
- Mutations under mutations field
- Things that are public availble for al consumers outside the scope of `viewer`

### Make it resiliant

- Think that your service might not be available so make things nullable that could be nullable in an outage.

## Want to make your own contract?

Fork this repo, use the [template](template.md) and raise a PR.
