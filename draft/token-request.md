# Request API token

Simple requests of API tokens at any server

## Purpose

Automate token requesting for developers for accessing an API. This will usually be the first flow that a client does in order to authorise itself with a server that supports contracts.

## Contract

```graphql
type TokenResponse {
  token: String!
  expires: DateTime!
}

type TokenChallenge {
  challenge: String!
  expires: DateTime!
}

interface TokenRequestMutations {
  requestTokenChallenge(identity: String!, signature: String!): TokenChallenge
  exchangeChallengeToken(
    challenge: String!
    signature: String!
  ): TokenResponse
}
```

## Implementation

Add to your query type:

```graphql
type Query implements TokenRequestRequests
```

Add to your mutation type:

```graphql
type Mutation implements TokenRequestMutations
```

## Request flow

1. Request a token challenge with your identity url and signature. This provides you the nonce that you need in order to prevent replay attacks.
2. Sign the token with your GPG private key and exchange for a short lived token back.

```graphql
mutation(
  $identity: String = "https://github.com/emilebosch.gpg"
  $signature: String = "-----BEGIN PGP SIGNED MESSAGE----- ...."
) {
  requestTokenChallenge(identity: $identity, signature: $signature) {
    challenge
  }
}
```

The identity is an https link to my GPG public key, that allows for verification that I am me. I sign the identity url with my own private key so that the server can already verify wheter my is correct and wheter it trusts the identity link. In order to mitigate replay attacks it needs to give you a challenge that can be signed, the challenge can only be used once.

You will need to sign the given challenge again and exchange that challenge for a API token:

```graphql
requestToken(challenge: "", signature: "") {
  token
}
```

## How to sign your tokens with GPG

Start with requesting a challenge and sign that with your key:

```bash
export GPG_TTY=$(tty); # needed on some OSX
echo "https://github.com/emilebosch.gpg" | gpg --clearsign | jq --slurp --raw-input
```

## Notes

- You request a challenge with your identity URL so that the server can already decide whether it wants to hand you out a challenge all. You need to sign it in order to get it verified. A server can decide itself whether to trust your gpg key. I.e using something like keybase is a good way.
- Tokens are short lived and need to be re-requested. This prevents stale and lingering tokens. The exchange itself is usually a matter of microseconds when you're already authorized.
- The server will send a 403 if you're not authorized
