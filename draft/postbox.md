# Postbox protocol

Simple server messaging

## Purpose

Make possible for sending messages to a consumer (site) like a support app such as intercom.

## Protocol

```graphql
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
type Query implements Postbox
```

Add to your mutation types:

```graphql
type Mutation implements PostboxMutatations
```

## Flow

A client can authenticate with the token-request flow if necessary and send a message directly.