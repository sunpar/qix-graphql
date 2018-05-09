# qix-graphql

> GraphQL Server on top of the Qlik Associative Engine (QIX).

[![CircleCI](https://img.shields.io/circleci/project/github/stefanwalther/qix-graphql.svg)](https://circleci.com/gh/stefanwalther/qix-graphql)

---

## Table of Contents

<details>

- [Motivation](#motivation)
- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [Roadmap](#roadmap)
- [Contribution](#contribution)
- [About](#about)
  * [Author](#author)
  * [License](#license)

_(TOC generated by [verb](https://github.com/verbose/verb) using [markdown-toc](https://github.com/jonschlinkert/markdown-toc))_

</details>

## Motivation

This **prototype** is following the idea of bringing the concept of [GraphQL](https://graphql.org/) together with the powerful Qlik Associative Engine (formerly also called QIX).

## Features

The prototype right now covers two different (complementary) scenarios:

- Getting information about one environment (e.g. listing all Qlik apps, etc.)
- Connecting to a single Qlik app working with this app, such as
  - Getting the data from an app
  - Being able to query the various tables & fields
  - Making selections
  - Creating on demand hypercubes
  - etc.

## Installation

```
$ git clone https://github.com/stefanwalther/qix-graphql
```

## Usage

Spin up the required containers using

```
$ docker-compose up
```

### Work with the environment scope

Open the GraphiQl UI: http://localhost:3004/graphiql

Get the list of docs:

```
query getDocs {
  docs {
    qDocId
    qDocName
    qConnectedUsers
    qFileTime
    qFileSize
    qConnectedUsers
  }
}
```

Retrieve a single doc:

```
query getDoc {
  doc(qDocId: "/docs/Consumer Goods Example.qvf") {
    qDocId
  }
}
```

Retrieve the fields of a doc:
```
query getDocFields {
  docfields(qDocId: "/docs/Consumer Goods Example.qvf") {
    qtr {
      qName
    }
  }
}
```

### Work with the app scope

If you get the meta data above from the entire environment and execute the following

```
{
  docs {
    qDocName
    qDocId
    qTitle
    _links {
      _doc
    }
  }
}
```

You'll see a list similar like this:

```
{
  "data": {
    "docs": [
      {
        "qDocName": "Consumer Goods Example.qvf",
        "qDocId": "/docs/Consumer Goods Example.qvf",
        "qTitle": "Consumer Goods Example",
        "_links": {
          "_doc": "http://localhost:3004/app/%2Fdocs%2FConsumer%20Goods%20Example.qvf/graphiql"
        }
      },
      {
        "qDocName": "Consumer Sales.qvf",
        "qDocId": "/docs/Consumer Sales.qvf",
        "qTitle": "Consumer Sales",
        "_links": {
          "_doc": "http://localhost:3004/app/%2Fdocs%2FConsumer%20Sales.qvf/graphiql"
        }
      }
    ]
  }
}
```

Copying the value of the attribute `_doc`, will give you the link to open another GraphiQl instance, *connecting to this single app*: 

This then allows you to get e.g. all content of a single table.

**Example:**

- Connect to `http://localhost:3004/app/%2Fdocs%2FCRM.qvf/graphiql`
- Then execute the following query to get all data from the "Account" table (`NOTE: RIGHT NOW ONLY WORKS WITH THE CRM.qvf APP` ;-)):

```
{
  account {
    AccountId
    Account_Rep_Name
    Account_Type
    Account_Billing_Street
    Account_Billing_City
    Account_Billing_State
    Account_Billing_Zip
    Account_Billing_Country
    Account_Industry
  }
}
```

## Roadmap

I have split the work in a few iterations:

- **Iteration 1: Core prototyping work** <== WE ARE HERE !!!
  - Focus on core functionality
  - Testing various ideas/concepts
  - No UI work
  - Little documentation
  - Messy code ;-)
- **Iteration 2: Some work on enhanced concepts**
  - JWT support / Section Access
  - Using multiple engines, using mira 
- **Iteration 3: Getting to a first clean version**
  - Cleaning up / refactoring
  - Automated tests
- **Iteration 4: Clean UI & additional functionality**

See [projects](https://github.com/stefanwalther/qix-graphql/projects) for more details.

## Contribution

Do you like the basic idea of this project? Hopefully!  
I am actively looking for other people who want to join this project ... Please [feel free to reach out to me](https://twitter.com/waltherstefan)

## About

### Author
**Stefan Walther**

* [twitter](http://twitter.com/waltherstefan)  
* [github.com/stefanwalther](http://github.com/stefanwalther) 
* [LinkedIn](https://www.linkedin.com/in/stefanwalther/) 
* [qliksite.io](http://qliksite.io)

### License
MIT

***

_This file was generated by [verb-generate-readme](https://github.com/verbose/verb-generate-readme), v0.6.0, on May 09, 2018._

