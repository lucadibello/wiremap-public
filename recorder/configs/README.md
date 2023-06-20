# Wiremap - Recorder microservice configuration manual <!-- omit in toc -->
Written by Luca Di Bello - luca.dibello@student.supsi.ch - 2022/2023 - Version 1.0.0  <!-- omit in toc -->

## Table of contents <!-- omit in toc -->

- [1. Default config file](#1-default-config-file)
- [2. Config file explained](#2-config-file-explained)
  - [2.1. `postgres`](#21-postgres)
    - [2.1.1. `host`](#211-host)
    - [2.1.2. `port`](#212-port)
    - [2.1.3. `username`](#213-username)
    - [2.1.4. `password`](#214-password)
    - [2.1.5. `database`](#215-database)
  - [2.2. `logger`](#22-logger)
    - [2.2.1. `level`](#221-level)
    - [2.2.2. `prefix`](#222-prefix)
    - [2.2.3. `logFilePath`](#223-logfilepath)
  - [2.3. `webserver`](#23-webserver)
    - [2.3.1. `graphqlPlaygroundEnabled`](#231-graphqlplaygroundenabled)
    - [2.3.2. `host`](#232-host)
    - [2.3.3. `port`](#233-port)
    - [2.3.4. `cors`](#234-cors)
      - [2.3.4.1. `origins`](#2341-origins)
      - [2.3.4.2. `methods`](#2342-methods)
      - [2.3.4.3. `maxAge`](#2343-maxage)
      - [2.3.4.4. `debug`](#2344-debug)
  - [2.4. `grpc`](#24-grpc)
    - [2.4.1. `host`](#241-host)
    - [2.4.2. `port`](#242-port)
    - [2.4.3. `tls`](#243-tls)
      - [2.4.3.1. `enabled`](#2431-enabled)
      - [2.4.3.2. `certFilePath`](#2432-certfilepath)
      - [2.4.3.3. `keyFilePath`](#2433-keyfilepath)
  - [2.5. `keycloak`](#25-keycloak)
    - [2.5.1. `url`](#251-url)
    - [2.5.2. `realm`](#252-realm)
- [3. Environment variables](#3-environment-variables)
  - [3.1. `POSTGRES_USERNAME` (required)](#31-postgres_username-required)
  - [3.2. `POSTGRES_PASSWORD` (required)](#32-postgres_password-required)

## 1. Default config file

```yaml
postgres:
  host: localhost
  port: 5432
  username: postgres
  password: postgres
  database: wiremap
logger:
  level: debug
  prefix: "[Recorder] "
  logFilePath: "logs/recorder.log"
webserver:
  graphqlPlaygroundEnabled: true
  host: "0.0.0.0"
  port: 49153
  cors:
    # Allowed origins
    origins:
      - http://localhost:3000
    # Allowed request methods
    methods:
      - POST
    # Max age of the preflight request
    maxAge: 5m
    # Debug mode
    debug: false
grpc:
  host: "0.0.0.0"
  port: 49154
  tls:
    enabled: true
    certFilePath: "./certs/service.pem"
    keyFilePath: "./certs/service.key"
keycloak:
  url: "http://localhost:8080"
  realm: "wiremap"

```

## 2. Config file explained

### 2.1. `postgres`

Configuration for the Postgres database. The recorder uses Postgres to store the data received from gRPC clients (scans and hosts from `decoder`).

#### 2.1.1. `host`

The host of the Postgres database.

#### 2.1.2. `port`

The port of the Postgres database.

#### 2.1.3. `username`

The username of the Postgres database.

#### 2.1.4. `password`

The password of the Postgres database.

#### 2.1.5. `database`

The name of the Postgres database.

### 2.2. `logger`

Configuration for the logger used by the recorder. The logger is based on the default Go logger ([log](https://pkg.go.dev/log)). It logs both to the console and to a file to allow for easy debugging.

#### 2.2.1. `level`

The log level of the logger. Possible values are `debug`, `info`, `warning`, `error` and `fatal`.

#### 2.2.2. `prefix`

The prefix that is prepended to every log message. For example, if the prefix is "`[Recorder] `", the log message "`Starting recorder`" will be logged as "`[Recorder] Starting recorder`".

#### 2.2.3. `logFilePath`

The path to the log file. The file will be created if it does not exist.

### 2.3. `webserver`

Configuration for the web server that exposes the GraphQL API.

#### 2.3.1. `graphqlPlaygroundEnabled`

Whether the GraphQL Playground should be enabled. The GraphQL Playground is a web interface that allows you to interact with the GraphQL API. It is useful for testing and debugging. Learn more about it [here](https://www.apollographql.com/docs/apollo-server/v2/testing/graphql-playground/).

#### 2.3.2. `host`

The host of the web server. This is the host that the web server will listen on.

#### 2.3.3. `port`

The port of the web server. This is the port that the web server will listen on.

#### 2.3.4. `cors`

Configuration for CORS (Cross-Origin Resource Sharing). CORS is a mechanism that allows restricted resources (e.g. fonts, JavaScript, etc.) on a web page to be requested from another domain outside the domain from which the first resource was served. Learn more about it [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).

##### 2.3.4.1. `origins`

A list of allowed origins. An origin is a combination of a protocol, domain and port. For example, `http://localhost:3000` is an origin. If you want to allow all origins, set this to `["*"]`.

##### 2.3.4.2. `methods`

A list of allowed request methods. For example, `["POST", "GET"]`.

##### 2.3.4.3. `maxAge`

The max age of the preflight request. This is the time that the browser should cache the preflight request. This is useful to reduce the number of preflight requests. Learn more about it [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Max-Age).

##### 2.3.4.4. `debug`

Whether debug mode should be enabled. If debug mode is enabled, the server will log every request and response. This is useful for debugging CORS issues.

Please note that debug mode should only be enabled in development environments. It should not be enabled in production environments.

### 2.4. `grpc`

Configuration for the gRPC server that exposes the gRPC API.

#### 2.4.1. `host`

The host of the gRPC server. This is the host that the gRPC server will listen on.

#### 2.4.2. `port`

The port of the gRPC server. This is the port that the gRPC server will listen on.

#### 2.4.3. `tls`

Configuration for TLS (Transport Layer Security). TLS is a cryptographic protocol that provides end-to-end security of data sent between applications over the Internet. Learn more about it [here](https://en.wikipedia.org/wiki/Transport_Layer_Security).

##### 2.4.3.1. `enabled`

Whether TLS should be enabled. If TLS is disabled, the gRPC server will not use TLS to secure the connection between the gRPC server and the gRPC client and to authenticate the gRPC client.

If TLS is enabled, you must provide a certificate and a key. The certificate and the key will be used to secure the connection between the gRPC server and the gRPC client and to authenticate the gRPC client.

##### 2.4.3.2. `certFilePath`

The path to the certificate file. The certificate file must be in PEM format. The certificate file must contain the certificate of the gRPC server and the certificate of the CA (Certificate Authority) that signed the certificate of the gRPC server. The certificate file must contain the certificate of the gRPC server first and the certificate of the CA second. The certificate file must not contain the private key of the gRPC server.

##### 2.4.3.3. `keyFilePath`

The path to the key file. The key file must be in PEM format. The key file must contain the private key of the gRPC server. The key file must not contain the certificate of the gRPC server or the certificate of the CA that signed the certificate of the gRPC server.

### 2.5. `keycloak`

Configuration for Keycloak. Keycloak is an open source identity and access management solution. The recorder uses Keycloak to authenticate users.

#### 2.5.1. `url`

The URL of the Keycloak server. For example, `http://localhost:8080`.

#### 2.5.2. `realm`

The name of the Keycloak realm. For example, `wiremap`. The recorder will only allow users from this realm to interact with the GraphQL API.

## 3. Environment variables

All the needed environment variables are specified via `.env` file. The `.env` file is a text file that contains the environment variables. The `.env` file is located in the root directory of the decoder.

To get started quickly, you can copy the `.env.example` file to `.env` and change the values of the environment variables in the `.env` file.

```bash
cp .env.example .env
```

### 3.1. `POSTGRES_USERNAME` (required)

The username of the Postgres database. The username is used by `docker-compose` to start the Postgres database.

### 3.2. `POSTGRES_PASSWORD` (required)

The password of the Postgres database. The password is used by `docker-compose` to start the Postgres database.
