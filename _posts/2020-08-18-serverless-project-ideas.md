---
layout:     post
title:      Serverless Project Ideas
date:       2020-08-18 18:00:00 -0700
summary:    Cool project ideas to play around with serverless development.
categories: aws serverless
---

Todo lists are boring, yet it seems to be the only project idea people can think
of online. So, everytime I think of a cool project idea, I post it here.

Sometimes I have already built it myself or have even created a tutorial. If
that's the case, I'll post links too.

---

## Ideas

1. [Contact Forms for Static Websites](#forms)
2. [Uptime Monitoring Service](#uptime)
3. [Congress Search Chatbot](#congress)
4. [Temporary email service](#email)


<a name="forms"></a>

### 1. Contact Forms for Static Websites

So you have to build a cool new website using a static site generator. Because of
that, you don't need to pay for an expensive server, and instead, use something
like GitHub pages or Netlify. But now you want to add a contact form.

Contact forms usually require a server to handle form submissions and trigger
an alert, telling you that someone reached from your form and what they said.
It would be cool if you could create a serverless app let you do something like
this:

```html
<form action="http://yourlambdafunction.com/form_identifier" method="POST">
  <input type="text" name="name">
  <input type="email" name="email">
  <textarea name="message"></textarea>
  <input type="submit" value="Send">
</form>
```

#### Features to Implement:
- App can create forms and track submissions
- Submissions are made by POSTing to a URL using a unique ID for the form
- Any form inputs are stored
- You should get alerted via email when anyone submits a form response

#### Bonus points:
- Add spam filtering
- Add the ability to redirect to a specified "thank you" page after a successful submission


<a name="uptime"></a>

### 2. Uptime Monitoring Service

Ever been surprised by a flood of emails from clients telling you your website
is down? Or worse, have you ever checked in on your website only to realize it
was down and you're not sure for how long?

An uptime monitoring service addresses this issue by checking your website at
regular intervals for its availability. If your website isn't responding
properly you should get notified via email, tweet, text message, or any other
communication method you prefer. This allows you to quickly identify any major
site braking issues quickly and fix them.

#### Features to Implement:
- App can be given a URL to check
- Checks are done at 5-minute intervals (or any regular interval you want)
- You are alerted when the URL returns anything other than a 200 level response
- You should get alerted via email when anyone submits a form response

#### Bonus points:
- Track and store successful checks and show off how durable the URL is
- Add multiple communication types (e.g. SMS, email, slack, tweets, etc.)


<a name="congress"></a>

### 3. Congress Search Chatbot

I want to find out who my congress representative is, but I have no idea where
to start. What a great problem to solve with a chatbot! Let's start with
Facebook messenger.

#### Features to Implement:
- Chatbot published to Facebook
- Takes an address as an input
- When a valid address is given, a list of names of congress representatives for that address is returned

#### Bonus points:
- Return contact info for each person too (e.g. phone, email, twitter, website, etc.)
- Allow people to ask for senators as well


<a name="email"></a>

### 4. Temporary email service

Spam is annoying. Advertising is annoying. People knowing my real email is
sometimes annoying. Sometimes I need an email to sign up to get a free PDF but
hate the flood of marketing emails that come after that. Let's create a
throw-away email service. It should be temporary, secure, anonymous, and
disposable.

#### Features to Implement:
- email accounts are easy to make and temporary
- all emails should be disposed of after the email account expires

#### Bonus points
- allow attachments to be sent and received
- allow different variations of expiry times
