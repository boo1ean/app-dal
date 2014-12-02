## app-dal

Part of app-helpers project.

Database access layer builder powered by [knex](http://knexjs.org/).

Since it uses knex it's fully promise based (powered by [bluebird](https://github.com/petkaantonov/bluebird))

## Installation

```
npm install app-dal
```

## Usage

```javascript
var dal = require('app-dal');
var knex = require('../your-knex');

var users = dal({ table: 'users', knex: knex });

users.query().then(console.log);
// [{ id: 1, email: 'email@example.com', ... }, ... ]
```

## API

### create (Object data)

Insert data in table and returns id of inserted row

```javascript
users.create({ name: 'John', email: 'jhon@example.com' }).then(console.log);
// 426
```

### update (Object data)

Data object should contain id property which is used for where closure.   
Update method returns updated row id.

Example of updating `email` for user with `id = 105` we do:

```javascript
users.update({ id: 105, email: 'new@example.com' }).then(console.log);
// 105
```

### find (Object|Int criteria)

Find single row by given criteria.

```javascript
users.find({ email: 'my@example.com' }).then(console.log);
// { id: 343, email: 'my@example.com', ... }
```

If scalar value is passed to `find` it assumes that it's row id and build corresponding where closure:

```javascript
users.find(343).then(console.log);
// { id: 343, email: 'my@example.com', ... }
```

### query (Object criteria, Object options)

Find list of objects by given criteria

```javascript
// Find all users
users.query();

// Find users of specific type
users.query({ type: 'manager' });
```

Also you can pass `offset` and `limit` params via `options` object

```javascript
// 50 users starting from 50 (typically for pagination)
users.query(null, { offset: 50, limit: 50 });
```

### remove (Int id)

Assumes that table has `removed_at timestamp` field (we do love soft deletes).

`remove` method simply inserts current timestamp into `removed_at` column

```javascript
// Remove by id
users.remove(304).then(console.log);
// 304

// Remove by criteria
users.remove({ name: 'John' }).then(console.log);
// 434
```

## LICENSE
MIT