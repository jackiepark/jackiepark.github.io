---
layout: post
title: diet your express router code
---

## First Commit

```js

// index
app.get('/api/component/:id', function (req, res) {
  axios.get(url+req.params.id)
    .then(function (id) {
      Model.find({id: id}, function (err, models) {
        if (err) {
          res.send(err);
        } else {
          res.send(models);
        }
      }
      res.send(result);
    }).catch(function (err) {
      res.send(err);
    });
}
// app.post ...
// app.put ...
// app.delete ...

```

## separate express router

```js

// index
api.use('/api/component', component);
// router
app.get('/:id', function (req, res) {
  axios.get(url+req.params.id)
    .then(function (id) {
      Model.find({id: id}, function (err, models) {
        if (err) {
          res.send(err);
        } else {
          res.send(models);
        }
      }
      res.send(result);
    }).catch(function (err) {
      res.send(err);
    });
}
// app.post ...
// app.put ...
// app.delete ...

```

## fat arrow function

```js

// router
router.get('/:id', (req, res) => {
  axios.get(url+req.params.id)
    .then(function (id) {
      Model.find({id: id}, (err, models) => {
        if (err) {
          res.send(err);
        } else {
          res.send(models);
        }
      }
      res.send(result);
    }).catch((err) => {
      res.send(err);
    });
});

```

## separate router and controller

```js

// router
router.route('/:id')
  .get(componentController.get)
  .put(componentController.put)
  .post(componentController.post)
  .delete(componentController.delete)
// controller
function get(req, res) {
  axios.get(url+req.params.id)
    .then(function (id) {
      Model.find({id: id}, (err, models) => {
        if (err) {
          res.send(err);
        } else {
          res.send(models);
        }
      }
      res.send(result);
    }).catch((err) => {
      res.send(err);
    });
}

```

## async await

```js

// controller
async function get(req, res) {
  try {
    const id = await axios.get(url+req.params.id);
    const result = await Model.find({id: id});
    res.send(result);
  } catch(err) {
    res.send(err);
  }
}

```

## remove tryCatch

```js

// router
let next = fn => (...args) => fn(...args).catch(args[2]);

router.route('/:id')
  .get(next(componentController.get))
  .put(next(componentController.put))
  .post(next(componentController.post))
  .delete(next(componentController.delete))
// controller
const id = await axios.get(url+req.params.id);
const result = await Model.find({id: id});
res.send(result);

```
