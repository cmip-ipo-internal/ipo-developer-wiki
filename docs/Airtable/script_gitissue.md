# Creating a GitHub Issue
In relation to a form submit or new value entry, we might want to perfrom an error check to ensure that our date is conformant, or accurate. 

Although it is possible to set up an automation to email, we may want a more visible solution - especially when we are working with an open source community - where the issue relates not to one individual or group. 

One way to do this it to create a github issue. 
The following script take form input and compiles it into a template, which can then be discussed, or ingested by a GitHub action. 

## Script
```js 
/* 

Airtable template made for the CMIP-IPO
All queries: contact the IPO Technical Officer: 
Daniel Ellis
daniel.ellis -@- ext.esa.int

Make sure that each label is '- ' delimited

*/
//////////////////////////////
// set info here
//////////////////////////////


// GitHub repository information
const owner = 'cmip-ipo-internal';
const repo = 'CVTool';
const githubToken = '******'; // Replace with your GitHub personal access token
const airtablename = 'cvtooltable'

const maintainors = {'Daniel Ellis':'wolfiex',};


//////////////////////////////
// gets the data from form
//////////////////////////////

let template;
const inputCode = input.get("")
// const table = base.getTable(airtablename); 

// Regular expression patterns to extract key-value pairs
const keyValuePattern = /<b>(.*?)<\/b><br\/>(.*?)<br\/><br\/><br\/>/g;
const keyPattern = /<b>(.*?)<\/b><br\/>/;
const valuePattern = /<br\/>(.*?)<br\/><br\/><br\/>/;

// Initialize an object to store key-value pairs
const result = {};

// Extract key-value pairs using regular expressions
let match;
while ((match = keyValuePattern.exec(inputCode)) !== null) {
  const keyMatch = keyPattern.exec(match[0]);
  const valueMatch = valuePattern.exec(match[0]);

  if (keyMatch && valueMatch) {
    const key = keyMatch[1];
    const value = valueMatch[1];
    result[key] = value.replace(/<[^>]*>/g, '');
  }
}



//////////////////////////////
// Log the resulting JSON object


result['Assign'] = result['Assign'].split('- ')
result['Label'] = result['Label'].split('- ').filter(d=>d)

result['Assign'] = result['Assign'].map(d=>maintainors[d]).filter(d=>d)


//////////////////////////////

template = `
---
# 🚀 ${result['Title']} 🚀
---

**Issuer**: ${'@'+ (result['Github'] || result['Email'].split('@')[0])}

**Related Issues**:
${JSON.stringify(result['Related'])||''}

**Description**:
${result['Description']}

**Steps to Reproduce** (if applicable):
${JSON.stringify(result['Reproduce'])||''}

**Expected Behavior**:
${JSON.stringify(result['Expected'])||'none'}

---

`


//////////////////////////////
// Submit github
//////////////////////////////

// Create a new GitHub issue
  const url = `https://api.github.com/repos/${owner}/${repo}/issues`;

  const requestBody = {
    title: result['Title'],
    body: template,
  };

  if (result['Label'])  requestBody['labels'] = result['Label']
  if (result['Assign']) requestBody['Assign'] = result['Assign']


  console.log(result)
  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: {
        'Authorization': `token ${githubToken}`,
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(requestBody),
    });
    if (response.status === 401) {
      console.log('no write on the token');
    }
    if (response.status === 201) {
      console.log('GitHub issue created successfully.');
    } else {
      console.error('Failed to create GitHub issue.',response);
    }
  } catch (error) {
    console.error('Error creating GitHub issue:', error);
  }
```