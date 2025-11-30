# Call Recording Disclosure With Twilio

There may be instances where you want to play audio (a recording) or a message with Twilio before a call connects - such as a message [disclosing](https://help.twilio.com/articles/360011522553) that the call is being recorded.

For incoming calls, this can easily be accomplished by putting a `<Say>` or `<Play>` verb in your TwiML before dialing the intended application or party. But for outgoing calls, how do you ensure a message or recording is played to someone as soon as they answer the call?

You can [send TwiML instructions to a recipient](https://www.twilio.com/docs/voice/twiml/number#example-5-running-twiml-before-parties-are-connected) that will be run before they're connected to the main call. This can be done through the `url` attribute of the `<Number>` verb.

> By setting the `url` attribute, we can specify a URL that will return a TwiML response to be run on the called party's end. This TwiML will run after they answer, but before the parties are connected.
>
> "Running TwiML Before Parties Are Connected" - twilio.com

```
<?xml version="1.0" encoding="UTF-8"?>
<Response>
    <Dial>
        <Number url="http://example.com/agent_screen_call">415-123-4567</Number>
    </Dial>
</Response>
```

In the example above, before the 415-123-4567 number being connected to the parent call, the `url` will be requested by Twilio and the response's TwiML will be executed.

You can have this URL call an endpoint on your server or if you're just looking for a simple message to be played, you can use a Twimlet.

```
https://twimlets.com/echo?Twiml=%3CResponse%3E%3CSay+voice%3D%22Polly.Ruth-Neural%22%3EThis+call+will+be+recorded+for+quality+assurance%3C%2FSay%3E%3C%2FResponse%3E
```

The URL above uses a Twimlet function, `echo`, with a corresponding URL encoded TwiML that will `<Say>` the message "This call will be recorded for quality assurance".

Altogether we end up with

```
<?xml version="1.0" encoding="UTF-8"?>
<Response>
    <Dial>
        <Number url="https://twimlets.com/echo?Twiml=%3CResponse%3E%3CSay+voice%3D%22Polly.Ruth-Neural%22%3EThis+call+will+be+recorded+for+quality+assurance%3C%2FSay%3E%3C%2FResponse%3E">415-123-4567</Number>
    </Dial>
</Response>
```

Now, when dialing out of our system, the 415-123-4567 number will hear our call recording disclosure message when they answer. After hearing the message, they'll be connected to the parent call and the two parties will then be able to speak to one another.

I hope this helps save someone time if they're looking to do something similar.


---

> Author: <no value>  
> URL: http://localhost:1313/2024/02/29/call-recording-disclosure-with-twilio/  

