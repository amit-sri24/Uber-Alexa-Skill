# Sample Phrases

```
Alexa, tell Uber to pick me up.

Alexa, ask Uber how far away the nearest car is.

Alexa, ask Uber where my car is.

Alexa, ask Uber if it is using sandbox. (Debugging Info)
```


# Setup

1. All configuraton and user-specific variables are handled inside of `config/default.json`. Copy `config/sample.json` to `config/default.json` and fill out the appropriate fields. You will need to provide all of your API keys. For Uber, you will need a valid token from oAuth2.0 which is currently up to you (see below in the README for how to do this). Optionally add a Forecast.io weather API key (`Weather.api-key`).

```js
{
  	"Uber": {
		"client_id": "required",
		"client_secret": "required",
		"server_token": "required",
		"redirect_uri": "optional",
		"access_token":"required",
		"sandbox" : true
	},
    "Alexa": {
		"location": { "latitude": required, "longitude": required },
		"lambda-arn" : "required"
    },
    "Weather" : {
    	"api-key" : "optional"
    }
}
```

2. Create a new Alexa skill and use the provided info in `interaction-model.txt` for your Intent Schema and Sample Utterences, as well as Custom Slot Types.

3. Upload code to your lambda function. `gruntFile.js` has been configured to upload to you Lambda function. You can deploy to Lambda using `grunt deploy`. See https://github.com/Tim-B/grunt-aws-lambda for more info

4. Configure lambda function. Make sure you set the handler to `skill.handler` and add an `Alexa Skills Kit` event source.

# Uber Authentication

Uber requires authentication using OAuth2 **which is a pain to deal with**.  I was able to get my auth credentials using [a blog post](http://uberhackathon.devpost.com/updates/3114-helpful-hints-for-hacking) from a Hackathon of theirs. They link to [sample code on GitHub](https://github.com/uber/Python-Sample-Application) for a Python OAuth library, but there is a bug in their code where they do not ask for the `request` permission which is needed to call an Uber. Until [my pull request](https://github.com/uber/Python-Sample-Application/pull/28) is accepted, you might want to run [my fork](https://github.com/objectiveSee/Python-Sample-Application/) of the library. 

## Getting your Uber auth token

1. Follow steps in their tutorial to get the Python app running. Note that on step 2 of their instructions, we want the `request` permission in addition to the `history` and `profile`.
2. Visit `http://localhost:7000` in your web browser, which will re-direct you to the Uber auth page and then re-direct you back to the localhost server. 
3. Once authenticated with Uber, you will see an auth token in the browser. **This is what you need** to use in the Alexa skill in order for Alexa to speak with the Uber API.

## Making this easier

This server could be shared with everyone wishing to use this skill. Perhaps someone can set it up on Heroku.

# Notes

- The main entry point for the code is `skill.js` which uses boilerplate Skill code from Amazon. This is where most of the Alexa and speech code lives.
- `uber-skill-handler.js` contains most of the code dealing with the Uber API.
- Provides a `sandbox` flag in config for testing.
