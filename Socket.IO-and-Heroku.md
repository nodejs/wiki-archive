Heroku and Socket.IO do not play well out of the box with each other. Socket.IO needs to have it's transport set to xhr-polling with a 10 second interval.

`io.set('transports', ['xhr-polling']);`   
`io.set('polling duration', 10);`

See this [Heroku article](https://devcenter.heroku.com/articles/using-socket-io-with-node-js-on-heroku/) for more information.