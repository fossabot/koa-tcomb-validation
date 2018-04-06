# koa-tcomb-validation
> tcomb validation as koa middleware

## Description
A koa middleware that checks `body/query` request matches with tcomb type, if not valid, return standard `400` http code, or can be intercepted via koa global error handling.

## API
### `validate(type, [location])`
#### type  
Type: `Function`  
a type defined with the tcomb library

#### location  
Type: `String`  
Default: `body`  
request property to compare with type object, eg `body/query`

## Usage
```js
const Koa = require('koa')
const bodyParser = require('koa-bodyparser')
const validate = require('koa-tcomb-validation')
const Router = require('koa-router')

const app = new Koa()
const router = new Router()

const body = t.struct({
  x: t.Number,
  y: t.Number
})

// request body will be check
router.post('/', validate(body, 'body'), async (ctx, next) => {
  ctx.status = 200
})

app.use(bodyParser())

// global middlewares
app.use(async (ctx, next) => {
  // the parsed body will store in ctx.request.body
  // if nothing was parsed, body will be an empty object {}
  ctx.body = ctx.request.body
  await next()
})

app.use(router.routes())
  .use(router.allowedMethods())

app.listen(3000)
```