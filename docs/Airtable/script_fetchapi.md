# Using the fetchAPI (currency converter)
Airtable is able to run scripts periodically or on a variety of triggers. In this case we write a script that checks the conversion rate between two currencies and updates a column of our table. 

! tip Note
    Due to limitations in the Airtable API we have to process and update our records in batches. In this example we use groupings of 50. 

## Script

```js 
console.log(`Hello, ${base.name}!`);

/* 

Airtable template made for the CMIP-IPO
All queries: contact the IPO Technical Officer: 
Daniel Ellis
daniel.ellis -@- ext.esa.int

Before usage the 
    - tablename, 
    - currency, 
    - amount, 
    - output field 
    - and last_updated 
columns need to be present, and correctly referenced within this script

Usage and ammendments to this script are permitted, but correct reference of the origin (this) and any changes are required. 

*/


////////////////////////////////////////////////////
// Configuration options to change!
////////////////////////////////////////////////////
const tablename = 'Per Diem';
const output_currency = 'EUR'
// fileds
const currency = 'Currency';
const amount = 'Per Diem amount';
const output_field = 'Per Diem in Euros';
const last_updated = 'Last updated';
////////////////////////////////////////////////////
const apiKey = '<need to enter key here>'

let table = base.getTable(tablename);
let conversions = {};

async function get_rate(currency){
    /* A function to get the latest conversion rate */

    const url = `https://api.apilayer.com/exchangerates_data/latest?base=${currency}`;
    
    
    if (conversions.hasOwnProperty(currency)){
        return conversions[currency]
    }else{

        // Fetch conversion rate from API - you could change this to any API you want
        let apiResponse = await fetch(url,{
            method: 'GET',
            headers: {
                'apikey': apiKey,
            }});
        let data = await apiResponse.json();
        console.log(data)


        console.log(data)
        let conversionRate = data.rates[output_currency];

        conversions[currency] = conversionRate || -1

        return conversions[currency]
    }

}

// retrieve the table
let result = await table.selectRecordsAsync({fields: [currency,amount]});

let inputConfig = input.config();
console.log(inputConfig);

const now = new Date().toISOString().split('T')[0];



// Create an array to hold update operations
let updateOperations = [];

// Prepare updates for each record
for (let record of result.records) {
    let currency_value = record.getCellValue(currency);
    let amount_value = record.getCellValue(amount);

    let conversion_value = await get_rate(currency_value);


    let updates = {
        id: record.id,
        fields: {
            [output_field]: amount_value * conversion_value,
            [last_updated]: `Conversion (${currency_value}): ${conversion_value} @ ${now}`
        }
    };

    updateOperations.push(updates);
}

// Update records in batches
const batchSize = 50; // Set the batch size according to your needs
for (let i = 0; i < updateOperations.length; i += batchSize) {
    let batch = updateOperations.slice(i, i + batchSize);
    await table.updateRecordsAsync(batch);
}

```