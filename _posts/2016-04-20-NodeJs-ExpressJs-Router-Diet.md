---
layout: post
title: diet your express router code
---

## First Commit

> 일반적으로 시작은 이렇습니다.

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

> router code 가 많아지면 도메인별로 라우터코드를 분리하게 됩니다.

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

> es6의 fat arrow function 을 이용하면 코드를 더 간결하게 만들 수 있습니다.

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

> router code 와 controller 코드를 분리함으로써 반복된 라우트 코드를 없애고 pure function 만 남겨
코드가독성이 더 높아졌습니다.

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

> callback 으로 인해 블록 depth 가 깊어져서 로직코드를 읽어내기가 불편합니다.
async await 을 사용하여 code를 flat 하게 만들어 줍니다.

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

> async await 사용시 error 처리를 위해 try catch 를 반복 사용하게 됩니다.
try catch 중복코드를 제거하기 위해 router 로 try catch 코드를 옮겨 줍니다.

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
