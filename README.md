# newman-debug

A guide on how to debug [Postman](https://www.postman.com) collection runs with [newman](https://learning.postman.com/docs/running-collections/using-newman-cli/command-line-integration-with-newman/).

Feel free to clone this repository to walk through an example:

```
git clone https://github.com/kevinswiber/newman-debug.git
```

Be sure to run `npm install` in the `newman-debug` directory before getting started.

## Usage

In a Postman pre-request or test script, add a [debugger statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger) to set a breakpoint.

### Breakpoint example

```js
pm.test('query params are returned in the response body', () => {
    const subject = {
        foo: 'hello',
        bar: 'world'
    };

    debugger;
    const response = pm.response.json();

    pm.expect(response.args).to.eql(subject);
});
```

### Debugging in Visual Studio Code

The launch configuration provided in [.vscode/launch.json](https://github.com/kevinswiber/newman-debug/blob/main/.vscode/launch.json) contains a single configuration named `Debug newman`.  This can be launched from VS Code's Run view.  See [Debugging in Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) for more information.

### Debugging with the Node.js debugging client

To debug directly in your terminal, run:

```
node inspect ./node_modules/.bin/newman run collection.json --script-timeout 1800000
```

Example debug session:

```
$ node inspect ./node_modules/.bin/newman run collection.json -- --script-timeout 1800000
< Debugger listening on ws://127.0.0.1:9229/e7249c52-c382-42d3-b79e-66f391f3e2c3
< For help, see: https://nodejs.org/en/docs/inspector
< Debugger attached.
Break on start in node_modules/newman/bin/newman.js:3
  1 #!/usr/bin/env node
  2
> 3 require('../lib/node-version-check'); // @note that this should not respect CLI --silent
  4
  5 const _ = require('lodash'),
debug> c
< newman
< newman-debug
< → GET /get
<   GET https://postman-echo.com/get?foo=hello&bar=world
< [200 OK, 755B, 478ms]
<   ✓  response status is 200 OK
break in [unknown]:12
 10         foo: 'hello',
 11         bar: 'world'
>12     };
 13
 14     debugger;
debug> n
break in [unknown]:13
 11         bar: 'world'
 12     };
>13
 14     debugger;
 15     const response = pm.response.json();
debug> n
break in [unknown]:15
 13
 14     debugger;
>15     const response = pm.response.json();
 16
 17     pm.expect(response.args).to.eql(subject);
debug> exec response.args
{ foo: 'hello', bar: 'world' }
debug> .exit
```

## Questions?

Feel free to post to the [issues list](https://github.com/kevinswiber/newman-debug/issues).

## License

MIT

Copyright © 2020 Kevin Swiber kswiber@gmail.com