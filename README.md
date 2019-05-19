@yodasws/node-oauth
===================
A simple oauth API for `node.js`. This API allows users to authenticate against OAUTH providers and thus act as OAuth consumers.

OAuth1.0A tested against [Twitter](http://twitter.com), [term.ie](http://term.ie/oauth/example/), TwitPic, Yahoo!, and [Ally Invest](https://www.ally.com/api/invest/documentation/getting-started/).

Rudimentary OAuth2 supported and tested against Facebook, GitHub, foursquare, Google, and Janrain.

It also has support for OAuth Echo, which is used for communicating with 3rd party media providers such as TwitPic and yFrog.

<ins>I've added support for streaming services</ins> (keeping the connection open and periodically receiving new data).

Installation
------------

```bash
yarn add @yodasws/node-oauth
```

```bash
npm install @yodasws/node-oauth
```

Examples
--------

To run examples/tests install Mocha:
```bash
npm install -g mocha` and run `$ mocha your-file-name.js
```

### OAuth1.0

```javascript
const oauth = require('oauth');

const api = new oauth.OAuth(
	config.request_token_url,
	config.access_token_url,
	config.application_key,
	config.application_secret,
	'1.0',
	null,
	'HMAC-SHA1'
);

api.get(
	'https://api.tradeking.com/v1/accounts.json',
	config.access_token,
	config.access_secret,
	(error, data, response) => {
		if (error) console.error(error);
		console.log(data);
	}
);
```

#### Streaming Response

```javascript
const stream = api.get(
	'https://stream.tradeking.com/v1/market/quotes.json',
	config.access_token,
	config.access_secret
	// Notice no callback function. Get response below
);

stream.on('response', (response) => {
	response.setEncoding('utf8');
	response.on('data', (data) => {
		console.log(data);
	});
});

stream.end();
```

### OAuth2.0

Test suite requires Mocha.

```javascript
describe('OAuth2', () => {
	const OAuth = require('oauth');

	it('gets bearer token', (done) => {
		const twitterConsumerKey = 'your key';
		const twitterConsumerSecret = 'your secret';
		const oauth2 = new OAuth.OAuth2(
			twitterConsumerKey,
			twitterConsumerSecret,
			'https://api.twitter.com/',
			null,
			'oauth2/token',
			null
		);
		oauth2.getOAuthAccessToken(
			'',
			{
				grant_type: client_credentials,
			},
			(e, access_token, refresh_token, results) => {
				console.log('bearer: ', access_token);
				done();
			}
		);
	});
});
```
