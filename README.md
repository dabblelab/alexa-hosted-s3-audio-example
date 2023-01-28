# Playing mp3 files in an Alexa-Hosted skill
Hey this is Steve at Dabble Lab and in this tutorial, we'll look at playing an mp3 file from an Alexa-Hosted skill's S3 bucket.

## Overview

When you create an [Alexa-Hosted](https://developer.amazon.com/en-US/docs/alexa/hosted-skills/build-a-skill-end-to-end-using-an-alexa-hosted-skill.html) skill, an Amazon S3 bucket is also created for hosting media files. But the files you store in that bucket are not accessible publicly. So, you can't just use the default file URL in your skill, you need a signed URL.

## Steps

To follow-along, you can [click this link to deploy this skill template](https://deploy.dabble.dev/deploy/v2/yojf15ci7h) to your Alexa console. Or, you can copy and past the code from the following steps.

### Step 1 - Create and Alexa-Hosted skill
 - Login to [developer.amazon.com/alexa/console](https://developer.amazon.com/alexa/console)
 - Create a new Alexa-Hosted skill for Node.js using the 'Hello World' template

### Step 2 - Upload an mp3 file to S3
 - From the `code` tab open S3
 - Upload an mp3 file into the Media folder 
 > NOTE: Make sure the file is formatted correctly. Not all .mp3 files will play in an Alexa skill. Check out the [Alexa Audio Converter](https://www.jovo.tech/audio-converter) from Jovo.Tech for easily converting audio files to work with your skills.

### Step 3 - Get the S3 Pre-Signed URL to play the file

When you create an Alexa-Hosted skill in the Alexa Developer Console using the "Hello World" template, the code will include a file named [util.js](./lambda/util.js). The `util.js` file can be used to get a signed url for .mp3 file saved in the S3 `./Media` folder. So start by requiring `util.js` by adding the following to the top of `index.js`.

```javascript
const util = require('./util.js');
```

Now we can use the `getS3PreSignedUrl` method in `util` to get a signed URL that will let Alexa play the .mp3 file. We'll do that using an SSML tag in the speech output.  

> NOTE: The URI that get generated by the `util.js` needs to be formatted a bit or you'll get an error. Specifically, you need to replace instances of `&` in the URL with `&amp;`. Note the `.replace(/&/g,'&amp;')` in the example below.

```javascript
const audioUrl = Util.getS3PreSignedUrl("Media/your-audio-file.mp3").replace(/&/g,'&amp;');
return handlerInput.responseBuilder
        .speak(`hello world with audio <audio src="${audioUrl}"/>`)
        .getResponse();
```

## Testing

At this point you should be able to deploy the changes to your Alexa-Hosted skill and hear the audio file play when the skill lanuches. 



