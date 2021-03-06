# Changelog

## 1.2.0

- upgrade `passport` to `^0.3.0`

## 1.1.5

- fix to not throw if `req.user` is already defined
- upgrade dependencies

## 1.1.4

- add `status` argument to authentication callback

## 1.1.3

- make internal `req` mock less error-prone

## 1.1.2

- redirect `req.app.get('trust proxy')` to Koa's `app.proxy` (#22)

## 1.1.1

- add `authInfo` to request mock

## 1.1.0

Make the `req` mock to inherit from Koa's `request` object before adding delegates for Node's request and Koa's context to it. This makes custom properties/methods added to Koa's request available to passport and its authentication strategies.

## 1.0.1

- add `flash` to the `req` mock

## 1.0.0

Using ES6 Proxy currently breaks debugging (see #17). Until this is fixed, the Proxy approach got replace by delegating a whitelist of possible used properties/methods to either Node's request, Koa's context or Koa's request.

Note: There is nothing special about this being `1.0.0`. The major version bump is just because the update could possible break something.

## 0.5.1

- re-add authenticated user to `req.user`

## 0.5.0

- internal improvements (neither modify Node's request nor Koa's request object by mocking the `req` object with a proxy that forwards reads to either Node's request object, Koa's request object or Koa's context)
- `--harmony-proxies` has to enabled now

## 0.4.0

- Add support for custom authentication methods, e.g.:

```js
public.post('/login', function*(next) {
  var ctx = this
  yield* passport.authenticate('local', function*(err, user, info) {
    if (err) throw err
    if (user === false) {
      ctx.status = 401
      ctx.body = { success: false }
    } else {
      yield ctx.login(user)
      ctx.body = { success: true }
    }
  }).call(this, next)
})
```

## 0.3.2

- add generator function names for Koa debugging purposes

## 0.3.1

- make ctx.login() yieldable

## 0.3.0

- adapt recent Koa API changes

## 0.2.0

- `passport 0.2.x compatibility