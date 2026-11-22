# Zillow Search Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape Zillow search results for homes for sale, rentals and recently sold properties: price, address, beds, baths, living area, lot size, home type, days on Zillow, coordinates and listing brokerage. Search by city, ZIP code or a pasted Zillow URL.

This repository has ready scripts for the [Zillow Search Scraper: Homes for Sale, Rent and Sold](https://apify.com/v0iddo/zillow-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/zillow-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "locations": [
    "Austin, TX"
  ],
  "listingType": "for_sale",
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `locations` | array | Cities, ZIP codes or neighborhoods such as Austin, TX or 78701. |
| `listingType` | string | Which Zillow listings to read for the locations above. |
| `searchUrls` | array | Search result pages copied from your browser, for example https://www.zillow.com/austin-tx/ or a filtered search. |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "zpid": "29511637",
  "address": "3601 Denbar Ct, Austin, TX 78739",
  "street": "3601 Denbar Ct",
  "city": "Austin",
  "state": "TX",
  "zipcode": "78739",
  "price": 1275000,
  "priceText": "$1,275,000",
  "beds": 4,
  "baths": 4,
  "livingArea": 4767,
  "lotArea": 49223,
  "homeType": "SINGLE_FAMILY",
  "status": "FOR_SALE",
  "daysOnZillow": 6,
  "latitude": 30.170856,
  "longitude": -97.85964,
  "brokerName": "Berkshire Hathaway TX Realty",
  "soldDate": null,
  "imgSrc": "https://photos.zillowstatic.com/fp/421d12fb5cb2433aa61dd3c4b014bcc6-p_e.jpg",
  "url": "https://www.zillow.com/homedetails/3601-Denbar-Ct-Austin-TX-78739/29511637_zpid/"
}
```

Fields: `zpid`, `address`, `street`, `city`, `state`, `zipcode`, `price`, `priceText`, `beds`, `baths`, `livingArea`, `lotArea`, `homeType`, `status`, `daysOnZillow`, `latitude`, `longitude`, `brokerName`, `soldDate`, `imgSrc`, `url`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/zillow-scraper').call({"locations": ["Austin, TX"], "listingType": "for_sale", "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~zillow-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"locations": ["Austin, TX"], "listingType": "for_sale", "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$1.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
