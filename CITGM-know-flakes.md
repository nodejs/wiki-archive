1. #### `winston@v2.3.1`
   __new platforms:__ [`rhel72-s390x`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/808/nodes=rhel72-s390x/testReport/junit/(root)/citgm/winston_v2_3_1/)  
   there is one test that is timing sensitive  
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
2. #### `serialport@v4.0.7`
   __new platforms:__ [`ubuntu1404-64`](https://ci.nodejs.org/view/Node.js-citgm/job/citgm-smoker/808/nodes=ubuntu1404-64/testReport/junit/(root)/citgm/serialport_v4_0_7/)  
   at least two tests can timeout:  
```
   1) SerialPortBinding #list returns an array:
      Error: timeout of 2000ms exceeded. Ensure the done() callback is being called in this test.
   
   2) SerialPort light integration .list:
      Error: timeout of 2000ms exceeded. Ensure the done() callback is being called in this test.
```js
    it('returns an array', function(done) {
      SerialPortBinding.list(function(err, data) {
        assert.isNull(err);
        assert.isArray(data);
        done();
      });
    });
```
```js
  it('.list', function(done) {
    SerialPort.list(done);
  });
```