# Functions
## Introduction
Wetsand functions are stateless functions that run on server-side.

> You might want to start by installing `wetsand` package that contains utility functions. Although it is not necessary but it aids in communicating with Wetsand API.
>
> ```yarn add wetsand```

## Getting Started
Your module uses same pattern as express middleware.

```javascript
module.exports = (req, res) => {
  res.json({
    message: 'Welcome from wetsand functions!'
  });
}
```

## Configure
Add following object to your wetsand configruation

```json
"functions": {
  "source": "functions",
  "list": [
    {
      "name": "read-info",
      "module": "read-info",
      "trigger": "HTTP",
      "triggerValue": "ANY",
      "timeout": 60,
      "overwrite": true,
      "alwaysUseLatest": true,
      "ignore": [
        "yarn.lock"
      ]
    }
  ]
}
```

## Deploying a Function
1. Generate wetsand token from [security](https://console.firewebkit.com/security#tokens) page and export it as `WETSAND_TOKEN`. Minimum scope required for hosting deployment is `/functions/deploy`
2. Now use [wetsand-cli](https://www.npmjs.com/package/wetsand-cli) to deploy:
```bash
yarn wetsand deploy-function -t "$WETSAND_TOKEN" --function read-info -m "My first function"
```
3. You can confirm the function is deployed by going to [functions page](https://console.firewebkit.com/functions)
4. You can now trigger the function by checking `Trigger` column in list of your functions (on function page).

## Cors
If you want to setup cors or other options for your function, you will need to initiate it as express app

```javascript
const express = require('express');

const app = express();
app.use(express.json());

app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', 'https://mydomain.com');
  res.header('Access-Control-Allow-Methods', 'POST');
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
  next();
});

app.use((req, res) => {
  res.json({
    message: 'Welcome from wetsand functions that can have custom routes!'
  });
});

module.exports = app;

```

## Trigger
The functions can be triggered using

1. HTTP call –– `triggerValue` needs to be one of `GET` / `POST` / `DELETE` / `PUT` / `ANY`

  If you send a `GET` request when `triggerValue` is `POST`, the request will result in `405 Method Not Allowed`. However, if you have `ANY`, all the HTTP methods are accepted). See HTTP section below for more details.
2. Schedule –– `triggerValue` needs to be [cron expression](https://docs.oracle.com/cd/E12058_01/doc/doc.1014/e12030/cron_expressions.htm) e.g, `1 0 * * *`

### HTTP
There are 3 types of HTTP function you can create. These are the values used by `trigger` field in firewebkit.json.

#### `HTTP`
Anyone can trigger these functions using an HTTP client.

#### `HTTP_AUTH`
These function require security token and passed in to query parameter when triggering the function. The security token must be scoped to `/authfn`

The security token is passed in with `firewebkit-token` parameter. For example `https://fn.firewebkit.com/abcdefghijklmnopqrstuvwxyz/save-user?firewebkit-token=<Security Token>`. In this example `abcdefghijklmnopqrstuvwxyz` is your project ID.

You can also put this function behind hosting options and pass in security token internally using hosting options. See [Example #4 - Function call with custom query](/services/hosting.md#rewrite-rules)

> See [tokens](/security/tokens.md) to these generate tokens

#### `HTTP_SCOPED_AUTH`
These functions require function specific security token and passed in to query parameter (just like `HTTP_AUTH`) when triggering the function. The security must be scoped to `/authfn/<function1>,<function2>,...` for example `/authfn/create-image,resize-image` will be able to trigger functions `create-image` and `resize-image`.

See `HTTP_AUTH` above for more details.

## Function box
The function box is an isolated environment that runs your code. It can be in one of 2 states:

1. Hibernate state where function box is sleeping and will be provisioned upon receiving the request
2. Idle state where function box is ready and waiting for triggers

## Environment Variables
Your functions can be provisioned with custom environment variables that you define on [functions page](https://console.firewebkit.com/functions) by clicking "Env" button. If your function box is running, it will need to be restarted for any changes to the environment variable to take place.

<img src="https://i.imgur.com/Is1ilki.png" style="width:200px;border:1px dashed #ccc;"/>

## View Logs
Once your function box is running, you can view the logs from your functions on [functions page](https://console.firewebkit.com/functions) by clicking "View Logs" button. Following example if from [Wetsand Demo project](https://github.com/amrayn/wetsand-demo).

<img src="https://i.imgur.com/4xelbjx.png" style="width:500px;border:1px dashed #ccc;"/>
