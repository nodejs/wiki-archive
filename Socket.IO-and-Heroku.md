Heroku and Socket.IO do not play well out of the box with each other. Socket.IO needs to have it's transport set to xhr-polling with a 10 second interval.

io.configure(function () {
	io.set('transports', ['xhr-polling']);
	io.set('polling duration', 10);
});