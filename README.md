# graphql-immutable-selector

This library provides an adapter between graphql query language and immutablejs maps. It ships with two functions:

* `graphqlImmutableSelector` - higher order function to create a state selector from a graphql query
* `getPathsFromAst` - function that parses a graphql AST (such as the one generated by `graphql/language/parser`) into an array of Immutable-friendly paths.

Currently it only supports selecting static properties, but I might add simple find functionality in the future.

## Usage

### As a Redux state selector

```
import { Component } from 'react';
import { connect } from 'react-redux';
import { graphqlImmutableSelector } from 'path-to-code';


class MyUserComponent extends Component {
  ...
}

const gql = 'query { user { name } }';
export connect(graphqlImmutableSelector(gql))(MyUserComponent);
```

### As a standalone to select from state

```
import { graphqlImmutableSelector } from 'path-to-code';
import { fromJS } from 'immutable';

const gql = 'query { user { name } }';
const getUser = graphqlImmutableSelector(gql);

const state = fromJS({
  user: {
    name: 'Ted',
    strengths: 'Speed, strength',
    weaknesses: 'Whiskey'
  }
});

const tedProperties = getUser(state);
```

### To parse GQL into an array of object paths

```
import { getPathsFromAst } from 'path-to-code';
import { parse } from 'graphql/language';

const gql = 'query { user { name friends { name }} }';
const paths = getPathsFromAst(parse(gql));
// -> [['user', 'name'], ['user', 'friends', 'name']]
```

## Installation

Probably don't use the bundled version right now. I've been messing around with Rollup, and it's currently including the full `graphql` source making this 80-line wonder clock in at a cool 8600 LOC.

I'd recommend copy/pasting until I get it properly packaged and bundled. I'll make it an NPM package then as well.

### Building

`npm run build` !!Using bundled output not recommended!!

### Running tests

`npm run test`

## TODO

* Fix module bundling
* Add support for querying functions

Inspired by the conversation [here](https://github.com/open-source-ideas/open-source-ideas/issues/4)

PRs welcome!
