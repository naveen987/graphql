# graphql

// importing graphql
var express = require('express');
var graphqlHTTP = require('express-graphql');
var { buildSchema } = require('graphql');

//second step is to build a schema using graphql

var schema = buildSchema(`
  type Query {
    quoteOfTheDay: String
    random: Float!
    rollThreeDice: [Int]
  }
`);

// "var schema" is where your creating a variable and assigining it to a schema
// "type Query" In the second step we are providing a query with types we can provide different type like float, boolean, string
// "!" mark defines that the type is not nullable
// "[int]" defines when we are using list like 
  type Query {
  rollDice(numDice: Int!, numSides: Int): [Int]
}
// we will pass within [datatype] as a list

var root = {
  quoteOfTheDay: () => {
    return Math.random() < 0.5 ? 'Take it easy' : 'Salvation lies within';
  },
  random: () => {
    return Math.random();
  },
  rollThreeDice: () => {
    return [1, 2, 3].map(_ => 1 + Math.floor(Math.random() * 6));
  },
};
//these are the resolver function provided for the Api end points. => !!!daut to be cleared!!!

var app = express();
app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(4000);
console.log('Running a GraphQL API server at localhost:4000/graphql');
