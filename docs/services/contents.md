# Contents Manager
You can manage your contents right from Wetsand console. The contents are then available from the Wetsand API.

## Pages & Segements
The page is high level content. For example, "about" is a page and in it are various segments like "copyright" and "background"

The segement must be specified even if you're using just the page. We recommend using `default` as segment name in such cases.

All the segements can be accessed via GET API call.

* `https://api.amrayn.com/<project_id>/contents/about` - this will display contents in following format:
```
{
  "copyright": {
    "contents": "<p>Copyright 2020 - Amrayn Web services</p>",
    "title": "Copyright"
  },
  "background": {
    "contents": "<p>Some contents here ...</p>",
    "title": "Our History"
  }
}
```

* `https://api.amrayn.com/<project_id>/contents/about/background` - this will display contents in following format:

```
{
  "contents": "<p>Some contents here ...</p>",
  "title": "Our History"
}
```


## Options
Following options can be set from the editing page

* Restricted: The contents can only be edited by Admin or the developer. Managers and editors cannot edit this content.
* Editable: This option is no longer used and will be removed in the future. This was replaced with "Restricted" option
* Published: The contents will be visible via API. If this is unchecked, the 404 error will be returned
