## Try/Catch blocks can be expensive inside functions

If you have a function that has performance critical code in it (particularly if it's allocating a lot of variables), then don't use a try/catch inside the function.  Catch the errors from outside the function instead.

    function test1(j) {
            try{
            	var s = 0;
            	for (var i = 0; i < j; i++) s = i;
            	for (var i = 0; i < j; i++) s = i;
            	for (var i = 0; i < j; i++) s = i;
            	for (var i = 0; i < j; i++) s = i;
            	return s;
            }
            catch(ex) {
            	console.log(ex);
            }
    }

    // Slow!
    test1(1000000);

    function test2(j) {
        var s = 0;
        for (var i = 0; i < j; i++) s = i;
        for (var i = 0; i < j; i++) s = i;
        for (var i = 0; i < j; i++) s = i;
        for (var i = 0; i < j; i++) s = i;
        return s;
    }

    // Much faster
    try {
        test2(1000000);
    } catch(ex) {
        console.log(ex);
    }

[See this mailing list thread for background](https://groups.google.com/forum/#!topic/nodejs-dev/E-Re9KDDo5w) and [this performance test case.](http://jsperf.com/try-catch-performance-overhead)