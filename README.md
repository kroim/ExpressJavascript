# express-opentracing

Middleware for express to enable opentracing.
Supports any opentracing tracer compatible with version 0.11.0 of the opentracing javascript library.

## Install
```
npm install --save express-opentracing
```

## Usage

E.g., using LightStep as your tracer:

```
import * as express from "express";
import middleware from "express-opentracing";
import * as LightStep from "lightstep-tracer";

const lsTracer = LightStep.tracer({
  access_token   : 'foo',
  component_name : 'bar',
});

const app = express();
app.use(middleware({tracer: lsTracer}));
```

To trace specific HTTP calls, or to exclude certain calls (e.g. hosted static content), modify `app.use(middleware())` above:

```javascript

app.use((req, res, next) => {
  // exclude paths that start with '/css' or '/js'
  if (req.path.startsWith("/css") || req.path.startsWith('/js')) {
    return next()
  }
  // trace calls
  middleware({tracer: tracer})(req, res, next);
});
```

## Options
The `middleware` function takes in an options object as its only argument.
```
const options = {
  tracer: [Tracer], // Defaults to the opentracing no-op tracer.
}
```
