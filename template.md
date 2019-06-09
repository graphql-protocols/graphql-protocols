# My Protocol

Simple but clear one line of the protocol

## Purpose

Multiline description of the purpose of the protocol

## Protocol 

Write the SDL here:

```graphql

# Write the graphql SDL
type Message {
  message: String
}

interface Postbox {
  messages: [Message]
}

interface PostboxMutatations {
  postMessage(message: String): Message
}
```

## Implementation

Add to your query type:

```graphql
type Query implements MyXYzProtocol
```

Add to your mutation types:

```graphql
type Mutation implements MyMutationProtocol
```

## Flow

Describe how the flow works