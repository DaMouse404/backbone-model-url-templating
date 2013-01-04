# backbone-model-url-templating
Plugin to Backbone.js to allow for model URL templating.

Model URL templating are a handy way to create models that have service urls that change depending on the model's data. This is useful for services that have complex URL patterns.

## Usage

URL templating allows for the URL of sync calls to be dynamically generated according to a template. By default the URL template is read from the `url` property of the model. A different URL for each sync method can be specified in the `urls` property as key/value pairs.

### Basic syntax

```javascript
var Model = Backbone.ServiceModel.extend({
	url: 'path/to/some/api/{id}.json'
});

var model = new Model({ id: 12345 });
model.fetch(); // will make service call to 'path/to/some/api/12345.json'
```

### Interpolation

The URL template uses the `{ property }` syntax. All values are automatically URL encoded and can be assumed safe. Any properties in the Model attributes or defined as static properties are available in the template. 

```javascript
'path/to/some/service/{id}.json' => 'path/to/some/service/12345.json'
```

The URL template may also do more complex interpolation since any valid javascript code may be executed within braces.

```javascript
'path/to/{ id ? id : "123" }' => '/path/to/123' or '/path/to/someId'
'path/to/{ name.split(" ").join(".").toLowerCase() }' => '/path/to/john.smith'
```

### Method level templating

The Backbone.Model calls sync with the different methods depending on the state of the model. This is described in [Backbone's Sync](http://documentcloud.github.com/backbone/#Sync) documentation. The `urls` property can be used to map the method call to a specific URL. If no method mapping is defined in the URLs property, it will default to the `url` property instead.

```javascript
urls: {
	'create': '/path/to/create/url',
	'read': 'path/to/read/url',
	'update': 'path/to/update/url',
	'delete': 'path/to/delete/url'
}
```

### URL function callbacks

Along with the string templating, URLs may also be in the form of a callback function. The function is called with the context of the current model and expects to return a string as the result. Since the result is processed through the interpolation engine, the result may also contain templating syntax.

```javascript
url: function (){ return this.get('name'); },

urls: {
	'create': function (){ return 'path/to/{id}'; }
}
```

## Support

To suggest a feature, report a bug, or general discussion: http://github.com/huffingtonpost/backbone-model-url-templating/issues/


