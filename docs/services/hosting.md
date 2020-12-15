# Hosting
## Introduction
Wetsand hosting service can be used to:
1. Host static website
2. Host SPA website with routes setup
3. Host domain names with rewrites to your [serverless function](/services/functions) (e.g, yourdomain.com/api/resize-image)

## Deploying a Site
Follow the following steps to deploy your site.
 1. First create target to deploy the site to. This can be done from [UI](https://console.firewebkit.com/hosting) by clicking "Add New Site"
    * "Target" is a string identifier for the source code (to point to) (e.g, `production`)
    * "Name" is name for your website (e.g, `www-redirect`)
    * "Commit Message" is a simple message that helps you as a developer.
 2. Write `firewebkit.json` and specify `hosting` object that looks like:
    ```json
    "hosting": {
      "target": "default",
      "name": "Default web page",
      "source": "build",
      "ignore": ["file.txt"]
    }
    ```
 3. Ensure you have "build" directory - this is the source of your website to be pushed
 4. Generate wetsand token from [security](https://console.firewebkit.com/security#tokens) page and export it as `WETSAND_TOKEN`. Minimum scope required for hosting deployment is `/hosting/deploy`
 5. Now use [wetsand-cli](https://www.npmjs.com/package/wetsand-cli) to deploy:
 ```bash
 yarn wetsand deploy-site -t "$WETSAND_TOKEN" -m "My first website"
 ```

 > Learn more about [tokens here](/security/tokens.md)

## Hosting Options
Hosting options determine how requests to your website are handled. This is an object with following high-level fields:

 * `ignore <array>` - Array of pattern (string) both glob and regular expressions that will be ignored
 * `forceHttps <boolean>` – Wetsand forces (and redirects) non-HTTP URLs to HTTPs
 * `parked <boolean>` – Responds with wetsand "parked" webpage for every request
 * `trailingSlash <boolean>` – Adds/removes trailing slash to the URLs if not found/found (respectively)
 * `headers <array>`
 * `rewrites <array>`

 ### Headers
 ```json
 "headers": [
    {
      "source": "**/*.@(jpg|jpeg|gif|png|js|css|ico)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "max-age=31536000"
        }
      ]
    }
  ],
 ```

 Any pattern matched to the `source` will get specified `headers` added to the request.

 ### Rewrite Rules
 Wetsand rewrites can be powerful and it can be dangerous if not correctly written.
 Each rewrite rule is an object with `source` which is a URL resolving pattern.

 Here you will find various examples that you can use to create your rewrite rules.

#### Example 1: Single Page Application

 ```json
 [
   {
      "source": "/(contact|why-us|services)(/?)(\\?.*)?",
      "destination": "/index.html"
    },
   {
     "source": "/\\?(.*)",
     "destination": "/index.html"
   },
   {
     "source": "/!(images|manifest.json|static|favicon|sitemap|robots.txt|assets)**",
     "destination": "/index.html",
     "patternType": "glob"
    }
 ]
 ```
 The example is best suitable for single page applications.
 1. `"source": "/\\?(.*)"` tells wetsand that anything with slash and `?` should use `index.html` as destination
 2. The source pattern in second object is `glob` pattern (determined by `patternType` which is only applicable with `destination`)
 3. The second pattern tells wetsand anything that doesn't match any of these files e.g, `manifest.json` should use `index.html` as destination

#### Example 2: Simple function call
 ```json
 [
   {
     "source": "/api/read-info",
     "function": "read-info",
     "acceptQueryParameters": true
   }
 ]
 ```

 This is a simple pattern matching that results in calling the function. For example,
 if your hosting domain is example.com, when you call `example.com/api/read-info` it will
 call `read-info` function, just as if you were to call `https://fn.firewebkit.com/<client-id>/read-info`.

 `acceptQueryParameters` means any query parameters in `/api/read-info` will be passed down to the function.

 Note: this would fail with query parameters because you have not told wetsand to handle it. You can change the
 `source` to `/api/read-info(.*)` to do it, but that would also mean even `/api/read-information` would work too.
 You can also use `/api/read-info\\?(.*)` - which would fix it but it would always require url to end with `?`.
 Unfortunately, this is current limitation, nevertheless, it doesn't affect the functionality.

#### Example 3: Function call with path
 ```json
 [
   {
      "source": "/api/query-people/:ageMin-:ageMax",
      "function": "read-info",
      "path": "/people\\?min=:ageMin&max=:ageMax"
    }
 ]
 ```
 Wetsand functions are very flexible and allow you to write single function with multiple routes. If you want to call
 that route with/or without query parameters, you can use this rewrite rule.

 This would send `req.query.min` and `req.query.max` with specified values. For example, `example.com/api/query-people/18-20`
 these values will be `18` and `20` as expected.

#### Example 4: Function call with custom query
 ```json
 [
   {
      "source": "/api/teenagers",
      "function": "read-info",
      "path": "/people",
      "query": {
        "min": 13,
        "max": 19
      }
    }
 ]
 ```
 This is similar to example 3 but with custom query parameter (that is an object instead of string). You can even use variables with it. For example
 ```json
   {
      "source": "/api/teenagers/:min",
      "function": "read-info",
      "query": {
        "min": ":min",
        "max": 19
      }
    }
 ```

 You can use this option to pass in `firewebkit-token` securily without exposing it to public.

 ```json
   {
      "source": "/api/teenagers/:min",
      "function": "read-info",
      "query": {
        "firewebkit-token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ3dm8xNm14..."
      }
    }
 ```

#### Example 5: Simple redirect
 ```json
 [
   {
      "source": "/@(donate)?(?)**",
      "redirect": "https://www.paypal.me/zuhd",
      "statusCode": 302,
      "patternType": "glob"
    }
 ]
 ```
 This would redirect the `example.com/donate` to `paypal.me/zuhd` with `302` status code

#### Example 6: API call
 ```json
 [
   {
     "source": "/api/:coreApi(cfg)(/?)(\\?.*)?",
     "api": ":coreApi"
   }
 ]
 ```
Calls list of APIs without needing to specify client ID. This essentially redirects to `https://api.firewebkit.com/<client-id>/<coreApi>` (uses internal host instead of external (i.e, `api.amrayn.com`)

If, for any reason, you need to use `apiv1` key instead of `api`.

```json
[
  {
    "source": "/api/:coreApi(cfg)(/?)(\\?.*)?",
    "api": ":coreApi"
  },
  {
    "source": "/api/:coreApi(captcha|enquiry)(/?)(\\?.*)?",
    "apiv1": ":coreApi"
  }
]
```

#### Example 7: Dump database records
```json
[
  {
     "source": "/api/data/:dataTable(people|places)(/?)(\\?.*)?",
     "dataTable": ":dataTable"
  }
]
```

With this rewrite rule, `example.com/api/data/people` or `example.com/api/data/places` would dump all the records of data table `people` and `places`.

If you want to access private data table, you will need to use `privateDataTable` key instead of `dataTable`. This will not work if the admin has disallowed
external access to the private data tables (using `DISABLE_INTERNAL_DATA_ACCESS` configuration)

#### Example 8: Read contents
```json
[
   {
     "source": "/api/contents/:page/(/?)(\\?.*)?",
     "page": ":page"
   },
   {
     "source": "/api/contents/:page/:seg/(/?)(\\?.*)?",
     "page": ":page",
     "segment": ":seg"
   }
]
```
You can read the contents from your contents editor for specified page/segement using `example.com/api/contents/mypage`
