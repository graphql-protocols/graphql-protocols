# MyXYzContract

Simple but clear one line of the contract

## Purpose

Multiline description of the purpose of the contract

## Contract 

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
type Query implements MyXYzContract
```

Add to your mutation types:

```graphql
type Mutation implements MyXYzContractMutations
```

## Flow

Describe how the flow works