# Filter Resource

A filter resource represents an instance of a filter inside MaxScale. Multiple\
services can use the same filter and a single service can use multiple filters.

### Resource Operations

#### Get a filter

Get a single filter. The _:name_ in the URI must be a valid filter name with all\
whitespace replaced with hyphens. The filter names are case-sensitive.

```
GET /v1/filters/:name
```

**Response**

`Status: 200 OK`

```
{
    "links": {
        "self": "http://localhost:8989/v1/filters/Hint-Filter"
    },
    "data": {
        "id": "Hint-Filter",
        "type": "filters",
        "relationships": {
            "services": { // All serices that use this filter
                "links": {
                    "self": "http://localhost:8989/v1/services/"
                },
                "data": [] // No service is using this filter
            }
        },
        "attributes": {
            "module": "hintfilter",
            "parameters": {} // Filter parameters
        },
        "links": {
            "self": "http://localhost:8989/v1/filters/Hint-Filter"
        }
    }
}
```

#### Get all filters

Get all filters.

```
GET /v1/filters
```

**Response**

`Status: 200 OK`

```
{
    "links": {
        "self": "http://localhost:8989/v1/filters/"
    },
    "data": [ // Array of filter resources
        {
            "id": "Hint-Filter",
            "type": "filters",
            "relationships": {
                "services": {
                    "links": {
                        "self": "http://localhost:8989/v1/services/"
                    },
                    "data": []
                }
            },
            "attributes": {
                "module": "hintfilter",
                "parameters": {}
            },
            "links": {
                "self": "http://localhost:8989/v1/filters/Hint-Filter"
            }
        }
    ]
}
```

<sub>_This page is licensed: CC BY-SA / Gnu FDL_</sub>

{% @marketo/form formId="4316" %}
