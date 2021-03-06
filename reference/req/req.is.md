# req.is()
Returns true if this request's declared "Content-Type" matches the specified media/mime `type`.  

Specifically, this method matches the given `type` against this request's "Content-Type" header.

### Usage
```js
req.is(type);
```


### Example
Assuming the request contains a "Content-Type" header, "text/html; charset=utf-8":
```javascript
req.is('html');
// -> true
req.is('text/html');
// -> true
req.is('text/*');
// -> true
```



<docmeta name="uniqueID" value="reqis371514">
<docmeta name="displayName" value="req.is()">

