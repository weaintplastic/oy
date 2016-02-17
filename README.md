# Oy [![npm version](https://badge.fury.io/js/oy-vey.svg)](http://badge.fury.io/js/oy-vey) [![Build Status](https://travis-ci.org/revivek/oy.svg?branch=master)](https://travis-ci.org/revivek/oy)

*Emails, oy vey!*

Render HTML emails on the server with React. Oy provides functionality to:

- Validate props against email best-practices with `Oy` components.
- Render templates server-side with `Oy.renderTemplate`.

[Blog Post](http://oyster.engineering/post/124868558323/emails-oy-vey-render-emails-with-react)

## Installation

```
npm install --save oy-vey
```

## Example usage

### Replace table markup with validating Oy components

```js
import React from 'react';
import {Table, TBody, TR, TD} from 'oy-vey';


export default React.createClass({
  displayName: 'BodyText',

  propTypes: {
    maxWidth: React.PropTypes.string.isRequired
  },

  render: function() {
    return (
      <Table width={this.props.maxWidth}>
        <TBody>
          <TR>
            <TD align="center">
              {this.props.children}
            </TD>
          </TR>
        </TBody>
      </Table>
    );
  }
});
```

### Compose higher level components like usual

```js
import React from 'react';

import MyLayout from './layout/MyLayout.jsx';

import BodyText from './modules/BodyText.jsx';


export default React.createClass({
  displayName: 'GettingStartedEmail',

  render: function() {
    return (
      <MyLayout>
        <BodyText>Welcome to Oy!</BodyText>
      </MyLayout>
    );
  }
});
```


### Inject rendered code into HTML skeleton with Oy.renderTemplate

For example, if using Express.js:

```js
import express from 'express';
import React from 'react';
import Oy from 'oy-vey';

import GettingStartedEmail from './templates/GettingStartedEmail.jsx';


const server = express();
server.set('port', (process.env.PORT || 8887));

server.get('/email/oy', (req, res) => {
  const template = Oy.renderTemplate({
    title: 'Getting Started with Foo',
    headCSS: '@media ...',
    bodyContent: React.renderToStaticMarkup(<GettingStartedEmail />),
    previewText: 'Here is your guide...'
  });
  res.send(template);
});

server.listen(server.get('port'), () => {
  console.log('Node server is running on port', server.get('port'));
});
```

## Default components

The `Oy` namespace exposes the following components validated against email best practices: 

```
Table TBody TR TD Img A
```

If you want to circumvent this validation, you can use `Oy.Element` and pass the `tagName` prop to implement your own validated element.

## HTML attributes

In addition to the [attributes supported by React](https://facebook.github.io/react/docs/tags-and-attributes.html#html-attributes), these HTML attributes are supported on Oy.Element components:

```
align background bgColor border vAlign
```

## Contributing

```
# Test
npm test

# Compile from ES6 in src/ to ES5 in lib/
npm run compile
```

We welcome contributions. If there's some information missing or ideas for how to make Oy better, please
send a pull request, file an issue, or email [1.vivekpatel@gmail.com](mailto:1.vivekpatel@gmail.com).

The best place to start would be in contributing new rules. [A running wishlist of email validation rules are in the Issues section](https://github.com/oysterbooks/oy/issues?q=is%3Aopen+is%3Aissue+label%3A%22rule+wishlist%22).
