# Data Manager
Firewebkit data is NoSQL database that gives developer power to create, update, filter, sort and paginate the documents without worrying about underlying mechanism. By default, firewebkit uses MongoDB API, but this does not affect how developers interact with their data.

## Create Data Table
Creating a data table requires you to have knowledge of [JSON Schema](https://json-schema.org/). However, Data table UI comes with templates that makes it easy for anyone with basic JSON knowledge to create data table smoothly.

You can use JSON to JSONSchema tools [like this one](https://www.liquid-technologies.com/online-json-to-schema-converter)

We're working on tools to build the forms from within the console.

## Firewebkit Functions Package
## Query

## Aggregation

## Count

## Update

## Security
### Public Access
### Private Data Key
### Users

## Data Types
### Date
#### Schema
Date field's schema will look like following

```json
{
  "date_joined": {
    "title": "Date Joined",
    "type": "string",
    "format": "date-time"
  }
}
```

#### Filter

```json
{
  "date_joined": {
    "$gte": "2020-01-12T20:15:31Z"
  }
}
```

For more details, refer to [MongoDB docs](https://docs.mongodb.com/manual/reference/operator/query/)

#### Read
When reading date, you can use one of the two popular libraries, dayjs or moment.js

```js
dayjs(person.date_joined).format('DD MMM YYYY')
```

### Text Area
#### Schema
```json
  "profile": {
    "type": "string",
    "title": "Profile",
    "maxLength": 2500
  }
```

#### UI Schema
```json
  "profile": {
    "ui:widget": "textarea",
    "ui:options": {
      "rows": 5
    }
  }
```
### Image
#### Schema
```json
"logo": {
   "type": "string",
   "title": "Logo",
   "description": "Logo must be 600x300"
 }
```
#### UI Schema
```json
"logo": {
  "ui:widget": "img",
  "ui:options": {
    "accept": "image/*",
    "upload": true
  }
}
```

### GeoJSON (Point)

#### Schema
```
"location": {
  "title": "Location",
  "type": "object",
  "properties": {
    "type": {
      "title": "Type",
      "type": "string",
      "default": "Point",
      "enum": [
        "Point"
      ]
    },
    "coordinates": {
      "type": "array",
      "title": "Coordinates",
      "items": [{
          "title": "Latitude",
          "type": "number"
        },
        {
          "title": "Longitude",
          "type": "number"
        }
      ]
    }
  },
  "required": [
    "type",
    "coordinates"
  ]
}
```

#### UI Schema
```
"location": {
  "type": {
    "ui:widget": "hidden"
  }
}
```
