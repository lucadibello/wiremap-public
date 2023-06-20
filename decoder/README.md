# Decoder <!-- omit in toc -->

A GoLang + GraphQL API microservice for decoding PCAP files and saving the packets into an Elasticsearch index for later analysis.

## Table of Contents <!-- omit in toc -->

- [1. Tech stack](#1-tech-stack)
- [2. Configuration](#2-configuration)
- [3. Directory structure](#3-directory-structure)

## 1. Tech stack

- [GoLang](https://golang.org/), GoLang is a statically typed, compiled programming language designed at Google by Robert Griesemer, Rob Pike, and Ken Thompson. It is a syntactically simple, compiled language with garbage collection, limited structural typing, memory safety features and CSP-style concurrent programming features added to the language.
  - [gqlgen](https://gqlgen.com/), gqlgen is a Go library for building GraphQL servers without any fuss. It works with any object graph, and has deep integration with the standard library and popular third party libraries.
- [GraphQL](https://graphql.org/), GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.
- [ElasticSearch](https://www.elastic.co/), Elasticsearch is a distributed, RESTful search and analytics engine capable of solving a growing number of use cases. As the heart of the Elastic Stack, it centrally stores your data so you can discover the expected and uncover the unexpected.
- [Docker Compose](https://docs.docker.com/compose/), Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.
- [gRPC](https://grpc.io/), gRPC is a modern open source high performance RPC framework that can run in any environment. It can efficiently connect services in and across data centers with pluggable support for load balancing, tracing, health checking and authentication. It is also applicable in last mile of distributed computing to connect devices, mobile applications and browsers to backend services.

## 2. Configuration

To configure the application in the right way, you need to create a `config.yaml` file in the root of the project. You can use the `config.yaml.example` file as a template.

```bash
cp config.yaml.example config.yaml
```

To learn more about the configuration of `Decoder` microservice, please read the configuration manual [here](./configs/README.md).

## 3. Directory structure

The directory structure of the `decoder` microservice is as follows:

```bash
.
└── decoder/ # Decoder microservice.
    ├── cmd/ 
    │   └── decoder # Decoder microservice entrypoint.
    ├── configs/ # Configuration files.
    │   ├── assets # README assets.
    │   └── elastic/
    │       └── indices # Directory containing the Elasticsearch index mappings.
    └── internal/
        ├── chi/ # Chi Router utilities. 
        │   └── middlewares # Custom Chi Router middlewares.
        ├── config # Package used to load the configuration.
        ├── converter # Package used for easy conversion between types.
        ├── elastic/ # ElasticSearch utilities.
        │   └── extensions # Extensions to the official Go client package.
        ├── graph/ # GraphQL utilities.
        │   ├── generated # gqpgen generated files.
        │   ├── pagination # GraphQL pagination utilities (following relay.dev standard).
        │   ├── resolvers # GraphQL resolvers.
        │   └── schemas # GraphQL schemas.
        ├── grpcclient # gRPC client.
        ├── keycloak # Keycloak authentication/authorization
        ├── logger # Custom logger.
        ├── mongo # Custom MongoDB client.
        ├── multipart # Multipart file upload utilities.
        ├── pcap/ # PCAP decoding utilities.
        │   └── mapper # Build network map from PCAP.
        ├── routes # Chi HTTP routes.
        ├── signer # Upload URL signer.
        └── splashscreen # Splashscreen utilities.
```
