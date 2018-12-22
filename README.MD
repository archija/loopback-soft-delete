Cascaded SoftDelete
=============

This module is designed for the [Strongloop Loopback](https://github.com/strongloop/loopback) framework version 3.x. 
It allows entities of any Model to be "soft deleted" by adding `deletedAt`, `deletedBy` and `_isDeleted` attributes. Thus helps projects which are using LoopBack version 3.x to maintain the version history without writting extra code for each model. 

This module is inspired from [gausie/loopback-softdelete-mixin](https://github.com/gausie/loopback-softdelete-mixin)

Queries following the standard format will no return these entities; they can only be accessed by adding `{ deleted: true }` to the query object (at the same level as `where`, `include` etc). This also archieve the cascade deleting of models throguh relations. 

Parent Context has been added to override the local transctions which used to cancel the current one. With the help of these, one can run the soft delete from their transctions and continue the same.

It is heavily inspired by [loopback-ds-timestamp-mixin](https://github.com/clarkbw/loopback-ds-timestamp-mixin).

Install
-------

```bash
  npm install --save loopback-softdelete-mixin
```

Configure
----------

To use with your Models add the `mixins` attribute to the definition object of your model config.

```json
  {
    "name": "Widget",
    "properties": {
      "name": {
        "type": "string",
      },
    },
    "mixins": {
      "cascadeSoftDelete" : true,
    },
  },
```

There are a number of configurable options to the mixin. You can specify alternative property names for `deletedBy`, `deletedAt` and `_isDeleted`, as well as configuring deletion to "scrub" the entity. If true, this sets all but the "id" fields to null. If an array, it will only scrub properties with those names.

```json
  "mixins": {
    "SoftDelete": {
      "deletedAt": "deletedOn",
      "_isDeleted": "isDeleted",
      "deletedBy": "deletedBy",
      "scrub": true,
    },
  },
```

Retrieving deleted entities
---------------------------

To run queries that include deleted items in the response, add `{ deleted: true }` to the query object (at the same level as `where`, `include` etc).
