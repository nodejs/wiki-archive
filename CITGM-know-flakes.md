1. `winston`: there is one test that is timing sensitive
```
An instance of winston.Logger with no transports the add() method with a supported transport the startTimer() method when not passed a callback
     ✗ should respond with the appropriate message 
         » expected true, got false // logger-test.js:193
```
```js
        "when not passed a callback": {
          topic: function (logger) {
            var that = this;
            var timer = logger.startTimer()
            logger.once('logging', that.callback.bind(null, null));
            setTimeout(function () {
              timer.done();
            }, 500);
          },
          "should respond with the appropriate message": function (err, transport, level, msg, meta) {
            assert.isNull(err);
            assert.equal(level, 'info');

            assert.isNumber(meta.durationMs);
            assert.isTrue(meta.durationMs >= 50 && meta.durationMs < 100);
          }
        }
```
2.