<p align="center">
  <big><strong>Cardano GraphQL</strong></big>
</p>

<p align="center">
  <img width="200" src=".github/images/cardano-logo.png"/>
</p>

<p align="center">
  <a href="https://jenkins.daedalus-operations.com/blue/organizations/jenkins/cardano-graphql/"><img src="https://jenkins.daedalus-operations.com/buildStatus/icon?job=cardano-graphql%2Fmaster&style=flat-square" /></a>
</p>

<hr/>

## Overview

Cross-platform, _typed_, and **queryable** API for Cardano. The project contains multiple
 [packages](./packages) for composing GraphQL services to meet specific application demands, 
 and a [docker-compose stack](./docker-compose.yml) serving the included [cardano-graphql-server Dockerfile](./Dockerfile) 
 and the extended [hasura Dockerfile](./packages/api-cardano-db-hasura/hasura/Dockerfile).
 The [schema](packages/api-cardano-db-hasura/schema.graphql) is defined in native `.graphql`,
 and used to generate a TypeScript [package](packages/client-ts/README.md) for client-side static typing.
 
  [Apollo Server](https://www.apollographql.com/docs/apollo-server/) 
  exposes the NodeJS execution engine over a HTTP endpoint, and includes support for open source metrics 
  via Prometheus, and implementing operation filtering to deny unexpected queries. Should you wish to have
  more control over the server, or stitch the schema with an existing service, consider importing the 
  executable schema from the `@cardano-graphql/api-*` packages only.

**GraphQL** is a query language and execution environment with server and client implementations
 across many programming languages. The language can be serialized for network transmission, 
 schema implementations hashed for assurance, and is suited for describing most domains.
 
**TypeScript** (and JS) has the largest pool of production-ready libraries, developers, and 
interoperability in the GraphQL and web ecosystem in general. TypeScript definitions for the 
schema, generated by [GraphQL Code Generator](https://graphql-code-generator.com), are available 
on [npm](https://www.npmjs.com/package/cardano-graphql-ts).

## Getting Started
``` console
git clone git@github.com:input-output-hk/cardano-graphql.git
cd cardano-graphql
```
### Build and Run via Docker Compose
Builds `@cardano-graphql/server` and starts it along with `cardano-node`, `cardano-db-sync-extended`, `postgresql`, and `hasura`:

``` console
docker-compose up -d --build && docker-compose logs -f
```
:information_source: _Omit the `--build` to use a pre-built image from Dockerhub (or locally cached from previous build)_
### Check Cardano DB sync progress
Use the GraphQL Playground in the browser at http://localhost:3100/graphql:
``` graphql 
{ cardanoDbMeta { initialized syncPercentage }}
```
or via command line:
``` console
curl -X POST -H "Content-Type: application/json" -d '{"query": "{ cardanoDbMeta { initialized syncPercentage }}"}' http://localhost:3100/graphql
```
:information_source: Wait for `initialized` to be `true` to ensure the epoch dataset is complete.
### Query the full dataset
```graphql
{ cardano { tip { number slotNo epoch { number } } } }
```
``` console
curl -X POST -H "Content-Type: application/json" -d '{"query": "{ cardano { tip { number slotNo epoch { number } } } }"}' http://localhost:3100/graphql
```
### :tada:
``` json
{ "data": { "cardano": { "tip": { "number": 4391749, "slotNo": 4393973, "epoch": { "number": 203 } } } } }
```

For more information, have a look at the [Wiki :book:](https://github.com/input-output-hk/cardano-graphql/wiki).

## How to install (Linux / Docker)

### Docker

See [Using Docker](https://github.com/input-output-hk/cardano-graphql/wiki/Docker).

### From Source 

See [Building](https://github.com/input-output-hk/cardano-graphql/wiki/Building).

## Documentation

| Link                                                                                               | Audience                                                     |
| ---                                                                                                | ---                                                          |
| [API Documentation](https://input-output-hk.github.io/cardano-graphql)                             | Users of the Cardano GraphQL API                             |
| [Wiki](https://github.com/input-output-hk/cardano-graphql/wiki)                                    | Anyone interested in the project and our development process |
| [Example Queries - Cardano DB Hasura](./packages/api-cardano-db-hasura/src/example_queries)        | Users of the Cardano DB Hasura API                             |

<hr/>

<p align="center">
  <a href="https://github.com/input-output-hk/cardano-graphql/blob/master/LICENSE"><img src="https://img.shields.io/github/license/input-output-hk/cardano-graphql.svg?style=for-the-badge" /></a>
</p>

## Deployments

### Graph QL

[Mainnet](https://cardano-graphql-mainnet.daedalus-operations.com/)
[Testnet](https://cardano-graphql-testnet.daedalus-operations.com/)

### Explorer App

[QA](https://explorer.shelley-qa.dev.cardano.org/en.html)
[Testnet](https://explorer.cardano-testnet.iohkdev.io/en.html)
