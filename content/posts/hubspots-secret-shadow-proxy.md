---
_eb_reusable_block_ids: []
_edit_last: "1"
_wp_old_date: "2020-12-15"
author: "Steven Wade"
categories:
  - Engineering
date: "2017-06-01T14:17:00+00:00"
guid: http://stevenwadejr.com/?p=29
parent_post_id: null
post_id: "29"
title: HubSpot's Secret Shadow Proxy
url: /2017/06/01/hubspots-secret-shadow-proxy/

---
I’ve got beef with the [HubSpot API](https://developers.hubspot.com/docs/overview) and it has its fair share of issues. I’ll do another post in the future discussing those more, but that’s not the focus of this post.

HubSpot has a secret, and I’ve found it out.

Ok, so it’s not the coolest secret in the world. It’s not like I’ve found a backdoor into their mainframe or detected a glitch in the Matrix or anything, but I thought it was pretty cool. So I’d like to share with the world.

I said I wasn’t going to mention the failings of the HubSpot API in depth in this post, and I’m not going to, but I should at least mention a few to serve as a prologue.

The HubSpot API is fragmented. What do I mean by that? Everything has an endpoint. That’s not necessarily bad, but when dealing with rate limits it can become a hassle. Having to get a list of things, then get a list of things for those things can easily get out of hand and you can run into a 1+n or an n+1 problem. For the requirements of the project that I’m working on, I worry about the 10 requests/second limit. A single page load won’t hit 10, but if multiple loads for different customers takes place, we might run into that problem.

This concern led me to google “hubspot graphql” in hopes that maybe, just _maybe_ there’s a better option out there that I hadn’t found yet.

There’s not.

However, that did lead me to this article on Medium: [Choosing GraphQL to build Drift’s messaging platform](https://medium.com/drift-engineering/choosing-graphql-to-build-drifts-messaging-platform-8b4310facbc1)

The author of that article states that he led the engineering team at HubSpot through their API rewrite. Before diving into the “lessons learned” and the confessions of what they did wrong, he starts out with this:

> At HubSpot, as soon as a front-end engineer would ask for another piece of data, especially one that required either calculation or aggregation, I would suggest they make one or more extra calls on the client.
>
> Unfortunately, they obliged.

This snippet sets up my favorite part of the article:

> Many of our front-end engineers decided to tackle the problem head-on and began deploying lightweight node.js proxy services to combine multiple requests from our APIs into simpler responses containing exactly what they needed while reducing network latency.

That got me thinking. I wanted to know what these guys had done. If they were dogfooding their subpar API, then maybe they had done something themselves to solve at least some of the problems that I was experiencing.

So I fired up my test HubSpot account, opened the network tab in the Chrome Developer Tools, and proceeded to snoop the network requests being made on a contact’s profile.

Most of the calls to the API were the same ones listed in the public docs, but there were a few in there that weren’t. Naturally I grabbed the Urls and threw them into Postman.

**Example URL:**  
`https://api.hubapi.com/email/v1/contacts/jdoe@example.com/subscriptions?hapikey=xxx-xx-xx-xxxxxx`

**Response:**

```
This endpoint does not accept EXTERNAL hapikeys, and has no special allowances for this hapikey (xxx-xx-xx-xxxxxx).  hapikey auth configuration for this endpoint: HapikeyAuthenticator{allows=INTERNAL}
```

Drats! But this was still interesting. They were using the same API subdomain, but certain URIs were marked as internal only. Fascinating.

I thought that was the end of that. I went about my business continuing work on on the task at hand - mucking about with API calls and transforming the responses for our needs. Then one day I notice something in the response object for a contact:

```
{
    "profile-url": "https://app.hubspot.com/contacts/{contact-id}/lists/public/contact/{a-super-long-obfuscated-string}/"
}
```

Clicking that link (the real one, not the sample :smiley: ) opens a public page showing most of the info for a contact that you’d see in the HubSpot app. “Is this truly public?” I thought to myself. So I opened it in an Incognito window and yup, same info. How was HubSpot doing this? Time to fire up the network tab again and go snooping!

This time, the snooping paid off. Most of the calls were all being made to the same endpoint: `https://api.hubapi.com/public-auth-proxy/v1/proxy?encryptedToken={the-same-super-long-obfuscated-string-from-before}`.

What the what?? I quickly googled the URL/URI combination and searched the HubSpot developer community for a sign that someone knows what this URL is. Nothing. No results found matching my query. What was this thing?

Looking at the rest of the request data, I see that they are all `POST` requests, with a body of their desired endpoint and any other parameters for the forwarding request.

```
{
    "uri":"https://api.hubapi.com/email/v1/contacts/jdoe@example.com/subscriptions",
    "method":"GET"
}
```

At this point I go “huh, I wonder” and I fire up Postman again. Guess what? It freaking worked. No `hapikey` or OAuth token needed. It seems that this proxy is authenticated via that “encrypted token” which is so readily passed around. As to how long those tokens last or whether they change, I can’t say, but what I can say is that I can now make (some) internal API requests.

I know what you’re thinking, _security hole_!!! Yes? But mostly no. After I found the proxy, I started testing all sorts of requests. Most of those internal API endpoints that I found earlier don’t work. It seems that the proxy is only set up to respond to certain pre-defined URIs, probably related to the data needed for the public profile page. Still wanting to see how far the power of the proxy went, I tried to `DELETE` a contact - nope. Error!

`Invalid path for /contacts/v1/contact/vid/{contact-id}`

So that’s good at least. There is a little bit of security in this thing.

One of the most useful URIs to be allowed by the proxy is to `/timeline/v2/contacts/{contact-id}`. It’s a wonderfully useful aggregate of info that if using the public API, would require multiple calls and client side parsing, ordering, and zipping.

At this point, I was feeling pretty clever and sufficiently satisfied with myself. Logically I knew that I couldn’t use the proxy in production as there’s no official public support and my code could break at any time - especially when they started noticing all of the new external traffic on that endpoint :smiley:.

So that’s where my journey ended. I’m no closer to API bliss than when I started, but I do feel a sense of joy for having discovered something that most people (if any?) in the general HubSpot API user public don’t know about. So I got that going for me, which is nice.

{{< figure src="/wp-content/uploads/sites/2/2020/12/which-is-nice.jpg" alt="" caption="" >}}
