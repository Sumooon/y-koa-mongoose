# y-koa-mongoose

## Features
* upgraded for Koa 2
* uses upgraded mongoose configured to use native `Promise`
* use with models
* use with schemas
* use with different database


## Examples

### With models

```js
const Koa = require('koa')
const mongoose = require('y-koa-mongoose')
const User = require('./models/user')
const app = new Koa()

app.use(mongoose({
    user: '',
    pass: '',
    host: '127.0.0.1',
    port: 27017,
    database: 'test',
    mongodbOptions:{
        poolSize: 5,
        native_parser: true
    }
}))

app.use(async (ctx, next) => {
    let user = new User({
        account: 'test',
        password: 'test'
    })
    await user.save()
    ctx.body = 'OK'
})

```

### With schemas

```js
const Koa = require('koa')
const mongoose = require('y-koa-mongoose')
const app = new Koa()

app.use(mongoose({
    username: '',
    password: '',
    host: '127.0.0.1',
    port: 27017,
    database: 'test',
    schemas: './schemas',
    mongodbOptions:{
        poolSize: 5,
        native_parser: true
    }
}))

app.use(async (ctx, next) => {
    let User = ctx.model('User')
    let user = new User({
        account: 'test',
        password: 'test'
    })
    //or
    let user = ctx.document('User', {
        account: 'test',
        password: 'test'
    })

    await user.save()
    ctx.body = 'OK'
})
```

### With database
```js
const Koa = require('koa')
const mongoose = require('y-koa-mongoose')

const app = new Koa()

app.use(mongoose({
    username: '',
    password: '',
    host: '127.0.0.1',
    port: 27017,
    database: ctx => {
        return ctx.headers['x-app']
    },
    schemas: './schemas',
    mongodbOptions:{
        poolSize: 5,
        native_parser: true
    }
}))

app.use(async ctx => {
    let user = ctx.document('User', {
        account: 'test',
        password: 'test'
    })

    await user.save()
    ctx.body = 'OK'
})
```

### With uri

```js
const Koa = require('koa')
const mongoose = require('y-koa-mongoose')
const app = new Koa()

app.use(mongoose({
    uri:'mongodb://<user>:<pass>@<host>:<port>/<db-name>',
    mongodbOptions:{
        poolSize: 5,
        native_parser: true
    }
}))

app.use(async (ctx, next) => {
    let User = ctx.model('User')
    let user = new User({
        account: 'test',
        password: 'test'
    })
    //or
    let user = ctx.document('User', {
        account: 'test',
        password: 'test'
    })

    await user.save()
    ctx.body = 'OK'
})
```

### With events

```js
const Koa = require('koa')
const mongoose = require('y-koa-mongoose')
const app = new Koa()

app.use(mongoose({
    uri:'mongodb://<user>:<pass>@<host>:<port>/<db-name>',
    mongodbOptions:{
        poolSize: 5,
        native_parser: true
    },
    events: {
        connected: ()=>{
            console.log('hello there')
        }
    }
}))
```

## Test
* mocha 会查找当前文件目录下test文件夹下的内容，自动执行
npm i mocha -g
mocha

## Licences

[MIT](LICENSE)
