# How to Scrape Cycle Trader in Node.js

This example calls the [Cycle Trader Listings Scraper](https://apify.com/piotrv1001/cycletrader-listings-scraper) through Apify's Node.js client. It uses an existing Actor; it does not implement a scraper.

## What this example does

- Calls the Actor with one Cycle Trader search URL
- Caps the run at ten listing records
- Waits for the run to finish
- Fetches the dataset and prints its rows

## Prerequisites

Node.js, an Apify account, and an Apify API token.

## Installation

```bash
npm install
```

## Environment setup

Copy `.env.example` to `.env` and replace the sample token with your Apify API token.

## Usage

```bash
npm start
```

The default URL gives you a quick data preview. For a useful price comparison, filter by make, model, condition, and location on Cycle Trader, then replace `startUrls[0].url` with the filtered search URL. Keep `maxItems` small at first.

## Code example

```js
import { ApifyClient } from 'apify-client';
import 'dotenv/config';

// Initialize the ApifyClient with your Apify API token
// Set APIFY_TOKEN in your .env file (copy .env.example to get started)
const client = new ApifyClient({
    token: process.env.APIFY_TOKEN,
});

// Prepare Actor input
const input = {
    startUrls: [{ url: 'https://www.cycletrader.com/motorcycles-for-sale' }],
    maxItems: 10,
    scrapeDetails: false,
};

// Run the Actor and wait for it to finish
const run = await client.actor("piotrv1001/cycletrader-listings-scraper").call(input);

// Fetch and print Actor results from the run's dataset (if any)
console.log('Results from dataset');
console.log(`💾 Check your data here: https://console.apify.com/storage/datasets/${run.defaultDatasetId}`);
const { items } = await client.dataset(run.defaultDatasetId).listItems();
items.forEach((item) => {
    console.dir(item);
});

// 📚 Want to learn more 📖? Go to → https://docs.apify.com/api/client/js/docs
```

## Example output

`sample-output.json` is illustrative, not a captured run. Basic rows can include `id`, `url`, `year`, `make`, `model`, `title`, `condition`, and `price`. Turn on `scrapeDetails` for fields such as mileage, location, dealer contact, photos, and specifications when the source listing provides them.

## Use cases

- Compare like-for-like motorcycle asking prices
- Track a dealer's public inventory over time
- Find newly listed bikes in a narrow search
- Review full listing details before contacting a dealer

## Try the actor on Apify

**[Open the Cycle Trader Listings Scraper on Apify](https://apify.com/piotrv1001/cycletrader-listings-scraper)**

## Related resources

- [How to Scrape Cycle Trader Motorcycle Listings and Compare Asking Prices](https://www.falconscrape.com/blog/how-to-scrape-cycletrader-motorcycle-listings)

## License

MIT
