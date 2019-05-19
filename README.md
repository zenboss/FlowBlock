# FlowBlock
A new kind of programming method.

```JavaScript
var FlowBlock = require('flowblock');
var flowblock = FlowBlock.create([
  (error, context, next) => {
    console.log('First flow block ran %o times', context.loopCounter);
    context.loopCounter += 1;
    next(error, context);
  },
  (error, context, next, end, pervious, first) => {
    if (context.loopCounter < 5) {
      first(error, context);
    } else {
      next(error, context);
    }
  },
]);
var initError = null;
var initContext = {
  loopCounter: 0,
};
function onFinish(error, context) {
  console.log('Flow is finished, error: %o, context: %o', error, context);
}
flowblock.start(initError, initContext, onFinish);
/**
 * output:
 * First flow block ran 0 times
 * First flow block ran 1 times
 * First flow block ran 2 times
 * First flow block ran 3 times
 * First flow block ran 4 times
 * Flow is finished, error: null, context: { loopCounter: 5 }
 */
```