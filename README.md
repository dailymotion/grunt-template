# grunt-template [![Build status](https://travis-ci.org/mathiasbynens/grunt-template.svg?branch=master)](https://travis-ci.org/mathiasbynens/grunt-template) [![Dependency status](https://gemnasium.com/mathiasbynens/grunt-template.svg)](https://gemnasium.com/mathiasbynens/grunt-template)

This Grunt plugin interpolates template files with any data you provide and saves the result to another file.

Since [`grunt.template.process`](http://gruntjs.com/api/grunt.template#grunt.template.process) is used for the templating, this Grunt plugin is very lightweight and doesn’t have any dependencies (other than Grunt itself).

## Getting started

This plugin requires Grunt v0.4.0+.

If you haven’t used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you’re familiar with that process, you may install this plugin with this command:

```bash
npm install grunt-template --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-template');
```

## The `template` task

### Overview

In your project’s Gruntfile, add a section named `template` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
	'template': {
		'options': {
			// Task-specific options go here
		},
		'your-target': {
			'options': {
				// Target-specific options go here
			},
			'files': {
				// Target-specific file lists go here
			}
		}
	}
});
```

### Options

The `options` property accepts the following options:

#### `data`
Type: `Object` or `Function`
Default: `{}`

This object contains the data that will be used while interpolating the template files. If you pass a function instead, it will be called when grunt-template needs the template data (lazy evaluation). This is useful if you want to load data from a file that is generated by another Grunt task, for example.

#### `delimiters`
Type: `String` or `Function`
Default: `config`

This is the delimiters' name that will be used to interpolate and evaluate code. A function that returns this name can be used too.
This property is useful when you want to generate JSP/ERB like code and you need the default interpolation delimiters to be `<%` and `%>`. See below for an example.

### Template syntax

Under the hood, grunt-template uses [`grunt.template.process`](http://gruntjs.com/api/grunt.template#grunt.template.process), which in turn relies on [Lo-Dash’s `_.template()` method](http://lodash.com/docs#template). Here’s a quick reminder of the default delimiters:

* Use `<%= value %>` to interpolate any values directly, i.e. inject them into the template without any modifications.
* Use `<%- value %>` to interpolate an HTML-escaped version of a given value. Use this if you’re generating an HTML file and you’re using unknown input data.

For more details and examples, see the [Lo-Dash’s API documentation for the `_.template()` method](http://lodash.com/docs#template).

### Usage example

Here’s a practical example of grunt-template. Here, the file `src/post.html.tpl` is loaded, then parsed as a template using the provided `data` object (with `title`, `author` and `content` properties), and finally the result is saved as `dist/post.html`.

#### `src/post.html.tpl`

```html
<!DOCTYPE html>
<title><%- title %></title>
<h1><%- title %>, by <%- author %></h1>
<p><%- content %></p>
```

#### `Gruntfile.js`

```js
module.exports = function(grunt) {
	grunt.initConfig({
		'template': {
			'process-html-template': {
				'options': {
					'data': {
						'title': 'My blog post',
						'author': 'Mathias Bynens',
						'content': 'Lorem ipsum dolor sit amet.'
					}
				},
				'files': {
					'dist/post.html': ['src/post.html.tpl']
				}
			}
		}
	});
	grunt.loadNpmTasks('grunt-template');
	grunt.registerTask('default', [
		'template'
	]);
};
```

#### `dist/post.html` (the end result)

```html
<!DOCTYPE html>
<title>My blog post</title>
<h1>My blog post, by Mathias Bynens</h1>
<p>Lorem ipsum dolor sit amet.</p>
```

#### `Gruntfile.js` (using a custom delimiter)

```js
module.exports = function(grunt) {
	grunt.template.addDelimiters('handlebars-like-delimiters', '{{', '}}')
	grunt.initConfig({
		'template': {
			'process-html-template': {
				'options': {
					'data': {
						'title': 'My blog post',
						'author': 'Mathias Bynens',
						'content': 'Lorem ipsum dolor sit amet.'
					},
					'delimiters': 'handlebars-like-delimiters'
				},
				'files': {
					'dist/post.html': ['src/post.html.tpl']
				}
			}
		}
	});
	grunt.loadNpmTasks('grunt-template');
	grunt.registerTask('default', [
		'template'
	]);
};
```

#### `src/post.html.tpl` (using a custom delimiter)

```html
<!DOCTYPE html>
<title>{{- title }}</title>
<h1>{{- title }}, by {{- author }}</h1>
<p>{{- content }}</p>
```


## Notes

If you use [gulp](https://github.com/wearefractal/gulp) instead of Grunt, but want to perform a similar task, use [gulp-template](https://github.com/sindresorhus/gulp-template).

## Author

| [![twitter/mathias](https://gravatar.com/avatar/24e08a9ea84deb17ae121074d0f17125?s=70)](https://twitter.com/mathias "Follow @mathias on Twitter") |
|---|
| [Mathias Bynens](https://mathiasbynens.be/) |

## License

grunt-template is available under the [MIT](https://mths.be/mit) license.
