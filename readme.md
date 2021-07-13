[![Open in Visual Studio Code](https://open.vscode.dev/badges/open-in-vscode.svg)](https://open.vscode.dev/bsnk-dev/pnwkit.gs)

<p align="center">
  <a href="https://github.com/bsnk-dev/pnwkit.gs">
    <img src="https://assets.bsnk.dev/pnwkit_icon_gs.png" alt="Logo" width="120" height="120">
  </a>

  <h3 align="center">PnWKit.gs</h3>

  <p align="center">
    Politics & War V3 API Library
    <br />
    <a href="https://bsnk-dev.github.io/pnwkit/"><strong>Explore the docs »</strong></a>
    <br />
    <br />
    <a href="https://www.npmjs.com/package/pnwkit">JS/TS Version</a>
    ·
    <a href="https://pypi.org/project/pnwkit-py/">Python Version</a>
    ·
    <a href="https://github.com/bsnk-dev/pnwkit/issues">Report Bug</a>
    ·
    <a href="https://github.com/bsnk-dev/pnwkit/issues">Request Feature</a>
  </p>
</p>


PnWKit is here to make interacting with the V3 Politics and War API easy. All you have to do is import the library, add your key, and make a query.

## Getting Started

This version of the PnWKit Library is for Google Apps Scripts so you can use the V3 API in spreadsheets easily. You will have to add it as a library using the library code.

### Prerequisites

Install the library using the code: ```1Of_sAY3Me77EKy14A5tQdcNY57SPC9KpI739YYio8t04XqkFSd0pPfjQ```.

## Usage

To use PnWKit just add your key, then you can make asyncronous queries.

```ts
pnwkit.setKey('xxxxx');

const nations = await pnwkit.nationQuery({id: [100541], first: 1}, `name`);

console.log(`Nation name: ${nations[0].name}`);
```

If you want to paginate your query for more results, just enable pagination after your query.

```ts
const nations = await pnwkit.nationQuery({id: [100541], first: 1}, `name`, true);

console.log(`Nation name: ${nations.data[0].name}, current page: ${nations.paginatorInfo.currentPage}`);
```

The queries are written in normal GraphQL, so you can get all the cities in a nation like this

```ts
const nations = await pnwkit.nationQuery({id: [100541], first: 1}, 
  `
  name,
  cities {
    name  
  }`);

console.log(`First city of ${nations[0].name}: ${nations[0].cities[0].name}`);
```

If you want to have multiple copies of PnWKit running at the same time, you can use the Kit class export.

```ts
const {Kit} = pnwkit;

const pnwkit = new Kit();
pnwkit.setKey('xxxx');

// queries...
```

### Caching

PnWKit has caching built right in for your convenience. 
Just create a cache function with ``PnWKit.cached`` and define how long it can be cached for. Then call it like normal.

Different calls return different cached versions too.

```ts
const cachableNationQuery = pnwkit.cached(
  pnwkit.nationQuery, // The query you want to cache
  1                   // How long that query can be cached at a time, in minutes
);

const nations = await cachableNationQuery({id: [100541], first: 1}, `name`);

// If you call it again within the age limit you'll get a cached version
const nationsCached = await cachableNationQuery({id: [100541], first: 1}, `name`);
```


You can also do the following queries in PnWKit:

- nationQuery
- allianceQuery
- tradePricesQuery
- tradeQuery
- warQuery
- treasureQuery
- colorQuery

You can look at the arguments and possible data to collect here at the [docs](https://bsnk-dev.github.io/pnwkit/).


# Development

## Testing

### Setup

Tests won't work right away, you have to specify private information inside the test/testconfig.json file.

It should look like this:

```json
{
  "apiKey": "xxxxx"
}
```

After that just run ```npm run test```

## Generating Docs

To generate documentation use the script: ```npm run docs```.
