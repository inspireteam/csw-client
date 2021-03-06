# csw-client [![CircleCI](https://circleci.com/gh/geodatagouv/csw-client.svg?style=svg)](https://circleci.com/gh/geodatagouv/csw-client)

> A very simple CSW client

[![npm version](https://badgen.net/npm/v/csw-client)](https://www.npmjs.com/package/csw-client)
[![dependencies Status](https://badgen.net/david/dep/geodatagouv/csw-client)](https://david-dm.org/geodatagouv/csw-client)
[![codecov](https://badgen.net/codecov/c/github/geodatagouv/csw-client)](https://codecov.io/gh/geodatagouv/csw-client)
[![XO code style](https://badgen.net/badge/code%20style/XO/cyan)](https://github.com/xojs/xo)

## Prerequisite

* [Node.js](https://nodejs.org) >= 8.0

## Features

* Fetch capabilities
* Fetch records
* Harvest (w/ Stream API)
* Support ISO 19139 (including Inspire profile)
* Support Dublin Core

## Installation

```js
npm install csw-client
```

## Usage

### Create a client

```js
const csw = require('csw-client');
const client = csw('http://your-csw-server.tld/csw', options);
```

#### Options

| Name | Description | Type | Default value |
| ---- | ----------- | ---- | ------------- |
| `userAgent`    | User-Agent string you want to use in requests   | `string` | `"CSWBot"` |
| `gzip`         | enable compression | `boolean` | `true` |
| `timeout`      | requests will fail after X ms | `integer` | _disabled_ |

### Harvest

#### Stream API

```js
client.harvest(options).pipe(outputStream);
```

#### Alternative

```js
client.harvest(options)
    .on('record', record => console.log(record.type))
    .on('error', err => console.error(err))
    .on('end', () => console.log('Finished!'))
    .resume();
```

#### Options

| Name | Description | Type | Default value |
| ---- | ----------- | ---- | ------------- |
| `step`      | number of records asked by `GetRecords` request | `integer` | `20` |
| `concurrency` | number of concurrent `GetRecords` requests | `integer` | `5` |

#### Events

| Name | Description | Properties |
| ---- | ----------- | ---------- |
| `record` | a new record is found | `type`: record type<br>`body`: [parsed value](https://github.com/sgmap-inspire/parsers) |
| `started` | harvesting has started | _none_ |
| `failed` | harvesting has failed | _none_ |
| `end` | harvesting has ended | _none_ |
