# Breaking Changes Between io.js Versions

## 2.0.0 from 1.x

### V8 Upgrade

The upgrade from V8 4.1 to 4.2 will require a recompile of all native add-ons. (The API surface area has not changed significantly, so most add-ons will "just work" after a recompile.)

### Parsed URL Representation Change

Parsed URL objects returned by the `url` module no longer have own data properties, but instead accessor properties on their prototype. This means that e.g. `parsedUrl.hasOwnProperty("href")` will now return false, and common object-copying methods (like Underscore/Lo-Dash's `_.extend` or ES2015's `Object.assign`) will not copy over the URL properties. To get back an object with the appropriate own data properties, use `parsedUrl.toJSON()`.