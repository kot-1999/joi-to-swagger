joi-to-swagger
==============

### Solving problem

In this fork I'm going to solve 'from to' problem with references. At the moment, the common implementation of this library generates wrong .json file. In case of 'from to' problem we can see that the "minimum" attribute contains an object instead of the reference or number. 

Commonly this library ignores Joi.ref() what is fine, but in this case it generates invalid .json which can't be compiled 


```json
{
  "durationFrom": {
    "type": "integer",
    "minimum": 0,
    "maximum": 999,
    "nullable": true,
    "example": 10
  },
  "durationTo": {
    "type": "integer",
    "minimum": {
      "adjust": null,
      "in": false,
      "iterables": null,
      "map": null,
      "separator": ".",
      "type": "value",
      "ancestor": "root",
      "path": [
        "body",
        "durationFrom"
      ],
      "depth": 2,
      "key": "body.durationFrom",
      "root": "body",
      "display": "ref:root:body.durationFrom"
    },
    "maximum": 999,
    "nullable": true,
    "example": 10
  }
}
```

Joi implementation example
```js
Joi.object({
  durationFrom: Joi.number().integer().min(0).max(999).optional().allow(null).example(10),
  durationTo: Joi.number().integer().min(Joi.ref('/body.durationFrom')).max(999).optional().allow(null).example(10)
})
```
