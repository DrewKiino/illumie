

# API Documentation

Refer to this json API [guidelines](http://jsonapi.org/)

Empty links and data are to be determined...

## Endpoints

### Authentication

#### Sign In

This endppoint should be able to take in either username or email

**POST** /auth/**signin**?username={*username*}&email={*email*}&pass={*password*}

```
{
	links: {},
	data: {[
		// the user model, this will expand later once I figure out
		// what we will need based on Facebook/Twitter user info etc.
		{
			id: xxx, // user's id unique to each object
			// access token should be unique to each sign in
			// if the user signs in again, then a new access token will be returned
			// invalidating the last one. Also, if the user signs out
			// then the access token is invalidated. For now, we'll only allow
			// one logged in device at a time
			accessToken: xxx,
			firstName: xxx,
			lastname: xxx,
			username: xxx,
			email: xxx,
			// password: xxx // included in database but not returned
			images: {
				profile: xxx // user profile image
			},
			subscriptionPlan: xxx, // "FREE", "PLUS", "PREMIUM"
			contacts: [
				// string array of user id's that are linked to this user
			]
		}
	]}
	status: 200
}
```

#### Sign Out

**POST** /auth/**signout**?user_id={*user's id*}

```
{
	links: {},
	data: {[]}
	status: 200
}
```

#### Sign Up

**POST** /auth/**signup**

```
// request body
{
	email: xxx,
	firstName: xxx,
	lastName: xxx,
	username: xxx,
	password: xxx
}
```

```
// Signing in will still be separate
// TBD
{
	links: {},
	data: {[]}
	status: 200
}
```

*All endpoints after this point will require an access token, we already know who the user is based on this token as well so we do not need to pass a user id*

### Subscription

#### List all subscriptions

**GET** /subscription/list?accessToken={*access token*}

```
{
	links: {},
	data: {[
		// subscription objects
		{
			type: "FREE",
			monthly: xxx // float or double, 2.99
			yearly: xxx// float or double, 29
			perks: [
				// string array of what the user is able to do
				"Ad-free", "Up to 5 channels", etc...
			]
		},
		{
			type: "PLUS",
			monthly: xxx,
			yearly: xxx,
			perks: [xxx]
		},
		{
			type: "PREMIUM",
			monthly: xxx,
			yearly: xxx,
			perks: [xxx]
		},
	]}
	status: 200
}
```

#### Subscribe

Based on what the user has sent, it will set the flag in the user's object. This will be the same route used when the user is subscribing for the first time or is simply modifying the current subscription.

**POST** /subscription/**subscribe**?type={*"FREE", "PLUS", "PREMIUM"*}?&accessToken={*access token*}

```
// TBD
{
	links: {},
	data: {[
		{
			// subscription object based on what the user subscribed to
		}
	]}
	status: 200
}
```

### Social Media

#### Link Account

/sm/**linkAccount**?type?={"FACEBOOK", "TWITTER", "SOUNDCLOUD", or "SPOTIFY"}&accessToken={access token}

```
// request body is dependent on the type parameter passed
// the server will then process the request body based on the type
// TBD
{
}
```

### Contacts

#### List all contacts 

This endpoint will return a list of all contacts that we can find after the user has linked any of his social media accounts. The data returned will be a list of user objects. 

The users have to exist in the database first. Meaning, they have linked their social media accounts that allow us to grab their specific id that we can check if they match any of the friend id's that we got from the user's owned linked accounts.

**GET** /contacts/list?accessToken={*access token*}

```
{
	links: {},
	data: {[
		// array of user objects
	]}
	status: 200
}
```

#### Add contacts

**POST** /contacts/add?accessToken={*access token*}

```
// request body
{
	users: [
		// string array of user id's
	]
}
```

```
// TBD
{
	links: {},
	data: {[]}
	status: 200
}
```

#### Onboard contacts

This endpoint will require third party API integration. The app will send a list of numbers to the server and the server will have to use an SMS API like [Twilio](https://www.twilio.com/docs/quickstart/php/sms/sending-via-rest) to sent individual SMS texts to these numbers.

**POST** /contacts/onboard?accessToken={*access token*}

```
// request body
{
	numbers: [
		// string array of mobile phone numbers
		// ex: "+1551231234", "+1553214321"
	]
}
```

```
// TBD
{
	links: {},
	data: {[]}
	status: 200
}
```

### Channels

#### List all channels

**GET** /channels/list?accessToken={*access token*}

```
{
	links: {},
	data: {[
		// array of channel objects
		{
			id: xxx, // channel id
			name: xxx, // String, ex: "Fader",
			// channel images will be shown in either square, banner, or large url images
			images: {
				square: xxx,
				banner: xxx,
				large: xxx
			}
		},
		{
			id: xxx,
			name: xxx,
			images: {
				square: xxx,
				banner: xxx,
				large: xxx
			}
		}
	]}
	status: 200
}
```

#### Subscribe to desired channels

**POST** /channels/**subscribe**?accessToken={*access token*}

```
// request body
{
	channels: [
		// string array of channel id's
	]
}
```

```
// TBD
{
	links: {},
	data: {[]}
	status: 200
}
```



































