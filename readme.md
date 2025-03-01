# winston-mailgun

[![NPM version](https://img.shields.io/npm/v/winston-mailgun.svg)](https://npmjs.org/package/package/winston-mailgun)
[![NPM Downloads](https://img.shields.io/npm/dm/winston-mailgun.svg)](https://npmjs.org/package/package/winston-mailgun)

This is a transport written to implement [mailgun-js](https://github.com/1lobby/mailgun-js) with [winston](https://github.com/winstonjs/winston)

## Installation

```bash
    npm install winston-mailgun
```

## Usage

```bash
    var winston = require('winston');
    require('winston-mailgun').MailGun;
    winston.add(winston.transports.MailGun, options);
```

## Winston MailGun accepts similar options as [mailgun-js](https://github.com/1lobby/mailgun-js)

Required:

- `apiKey` Your Mailgun API KEY  
- `domain` Your Mailgun Domain  
- `to` The email address(es) to receive e-mail  

Optional:

- `from` The address you want to send from. (default: winston@[server-host-name])
- `subject` Subject of e-mail
- `level` Level that this transport should log (e-mail).
- `silent` Boolean flag indicating whether to suppress output and thus, the e-mail.
- `proxy` The proxy URI in format http[s]://[auth@]host:port. ex: 'http://proxy.example.com:8080'  
- `timeout` Request timeout in milliseconds  

## Sending Attachments

Please see [Mailgun Attachments](https://github.com/1lobby/mailgun-js#attachments) and below examples.

You can send attachments the same way you normally would using mailgun-js. Simply pass an object into your meta-data with the property `attachment`.

## Examples

```js

var logger = require('winston');
require('winston-mailgun').MailGun;

var options = {
    apiKey: <your api key>,
    domain: <your domain>,
    to: test@email.com,
    from: test@example.com,
    subject: 'This is a Test Email',
    level: 'notify'
}

winston.add(winston.transports.MailGun, options);
var filename = 'mailgun_logo.png';
var filepath = path.join(__dirname, filename);
var fileStream = fs.createReadStream(filepath);
var fileStat = fs.statSync(filepath);

var attachment = {
  data: fileStream,
  filename: 'my_custom_name.png',
  knownLength: fileStat.size,
  contentType: 'image/png'
}

// To send an attachment with meta data (usually to send images)
logger.notify('This will be in the body of your email.', {
    attachment: attachment
});

// To send a normal file (no need for meta data, you can just use a string)
logger.notify('This will be in the body of your email.', {
    attachment: __dirname + '/' + filename
});

// No attachment, you just want to e-mail the objects in the body of the email
var testObject = {
    "id": 123123,
    "message": "This important thing needs to be e-mailed!"
}
logger.notify('This will be in the body of your email.', testObject)

// NOTE: If your object contains the property 'attachment', but 
// the attachment object does not contain 'data', the object will 
// just simply be appended to the body of your e-mail.

// Will NOT result in an e-mail with attachment
var testObject = {
    "attachment": "unknown",
    "test": "Email"
}
logger.notify('This will be in the body of your email.', testObject)

```
