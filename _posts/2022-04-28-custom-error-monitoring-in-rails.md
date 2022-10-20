---
layout:     post
title:      Custom Error Monitoring in Rails
date:       2022-04-28 22:00:00 -0400
categories: Ruby on Rails, JavaScript
---

## Inspiration

Recently I discovered Pieter Levels and the indie maker/startup community.

*Side note: this is a really cool scene and I would recommend checking them out. Links at the bottom ;)*

In a [tweet recently](https://twitter.com/levelsio/status/1517241274822447104), Pieter mentioned that he has rolled his own error monitoring system using a chat app, PHP, and some JavaScript. I found this so sick! And building out my own side projects, I wanted a cheap and simple solution to tracking bugs on my Rails project.

So, I took inspiration from Pieter and threw together my own solution using Trello, Rails, and some JavaScript.

## Steps

You want to replicate what I've done? The simplest way is to follow the these steps in order. Technically my journey to this solution was a lot more convoluted, filled with back-steps and rewrites, and done in the order I found most interesting at the time... but with this blog post I've boiled it down to the most important and straight forward steps.

### 1. Implement the Exception Notification gem in Rails

Get up to date gem version here: [https://rubygems.org/gems/exception_notification](https://rubygems.org/gems/exception_notification).

```ruby
# Gemfile.rb

# ...
gem 'exception_notification', '~> 4.5'
```

DONE!... kind of. There's more configuration shown in [the gem's README](https://github.com/smartinez87/exception_notification), but since this solution will not be using email, or any of the included notifiers, the rest of the configuration will be done when we add Trello integration.

### 2. Use Trello for error notifications

Add the `ruby-trello` gem so that we can create cards. Same thing as before, get the most up to date gem version here: [https://rubygems.org/gems/ruby-trello](https://rubygems.org/gems/ruby-trello).

```ruby
# Gemfile.rb

# ...
gem 'ruby-trello', '~> 3.1'
```

Next, [the gem's README](https://github.com/jeremytregunna/ruby-trello) has some good instructions for getting your API keys. Once you get those, add them to the encrypted Rails credential file:

```sh
# I use vi, but "nano", "atom --wait", and "code --wait" also work here
EDITOR="vi" bin/rails credentials:edit
```

```yaml
# Unencrypted config/credentials.yml.enc
trello_developer_public_key: TRELLO_DEVELOPER_PUBLIC_KEY
trello_member_token: TRELLO_MEMBER_TOKEN
```

Then we'll use those in initializing the `ruby-trello` gem in a new file:

```rb
# config/initializers/trello.rb
require 'trello'

Trello.configure do |config|
  config.developer_public_key = Rails.application.credentials.trello_developer_public_key
  config.member_token = Rails.application.credentials.trello_member_token
end
```

Now, the Exception Notification gem doesn't at this time have a Trello notifier, so we get to create one! I based mine off of the [Google Chat Notifier](https://github.com/smartinez87/exception_notification/blob/11c1e919ea50a1dccc3b2db515152e859c6dbebc/lib/exception_notifier/google_chat_notifier.rb) already included with the gem.

```ruby
# lib/exception_notifier/trello_notifier.rb
module ExceptionNotifier
  class TrelloNotifier
    def initialize(options)
      # do something with the options...
      @list_id = options[:list_id]
    end

    def call(exception, options={})
      formatter = Formatter.new(exception, options)
      create_card(exception, formatter, options)
    end

    private
      def create_card(exception, formatter, options)
        list = Trello::List.find(@list_id)
        card  = Trello::Card.new list_id: list.id,
          name: title(exception), desc: body(exception, formatter, options)
        card.save
      end

      def title(exception)
        exception.message
      end

      def body(exception, formatter, options)
        text = [
          "\nApplication: **#{formatter.app_name}**",
          formatter.subtitle,
          '',
          formatter.title,
          "- #{exception.message.tr('`', "'")}*"
        ]

        if (request = formatter.request_message.presence)
          text << ''
          text << '**Request:**'
          text << request
        end

        if (backtrace = formatter.backtrace_message.presence)
          text << ''
          text << '**Backtrace:**'
          text << backtrace
        end

        if data = options[:data]
          text << ''
          text << '**Data:**'
          text << data
        end

        text.compact.join("\n")
      end
  end
end
```

Other than helping me track down the API keys needed to make this integration work, I found this gem's README instructions somewhat lacking. Especially when it came to creating cards. But, after some digging through the project repository I found this helpful issue: [https://github.com/jeremytregunna/ruby-trello/issues/303](https://github.com/jeremytregunna/ruby-trello/issues/303).

To make sure this file get's loaded we are going to need to update the `config/application.rb` file to auto include files in the `lib` directory:

```ruby
# config/application.rb

#...
module SlimNewsletter
  class Application < Rails::Application
    # ...
    config.eager_load_paths << Rails.root.join("lib")
  end
end
```

Last up for this part, we need to configure our application to use the Exception Notification gem, and for the gem to use our custom Trello notifier. This is done in the `config/environments/production.rb` file, but while testing this out I temporarily had it in the `config/environments/development.rb` file. Keep in mind though, if you leave that there you can end up getting a ton of noisy Trello cards 😅.

```ruby
# error notifications

#...
Rails.application.configure do
  #...

  config.middleware.use ExceptionNotification::Rack,
    trello: { list_id: 'your-list-id' },
    error_grouping: true
  end
end
```

A couple of notes on that previous code sample. The `error_grouping` option is super helpful if you don't want to get spammed by redundant error messages. It's backed by `Rails.cache` by default, so it would be helpful to understand that a bit as well.

Another thing you may have noticed is the `your-list-id` value. How do you find that out? Well, I had the same issue until I found [this helpful article](https://digitalcommunications.wp.st-andrews.ac.uk/2021/12/15/adding-cards-with-the-trello-api/) by Nick Mullen. Trello has this cool JSON endpoint allowing you to find out information about your boards: `https://trello.com/b/[Your short board id found in the URL]/reports.json`.

Going to that address, you can find a list of the lists that are included on the board you want to make cards for. Just grab the value next to the `id` key of the list and add it as the value for the `list_id` in our configuration.

At this point, start or refresh your rails server, and errors will now be sent to Trello with info on the request, backtrace, error details, and any custom data (we'll get to that last one later)!

If you're anxious to test it out, and you've configured the integration in the development environment, you can add a `raise Error.new("whoops!")` call to one of your controllers and then visit the corresponding page locally.

### 3. Implement JavaScript error monitoring

So far we are able to track server-side errors, but most error monitoring services like [BugSnag](https://bugnag.com) or [Sentry](https://sentry.io) allow you to track client-side front-end errors as well. Here comes `window.onerror` to the rescue.

That JavaScript global property can be used to catch all JavaScript errors on your site. Paired with some AJAX and a custom logs controller, we can pass those errors to the Exception Notification gem and get Trello cards added for those too.

First up, writting the JavaScript:

```javascript
// app/javascript/application.js

// ...
window.onerror = function (message, source, lineNumber, columnNumber, error) {
  fetch("/logs/js", {
    method: "POST",
    headers: {
      "X-CSRF-Token": document.querySelector("[name='csrf-token']").content,
      "Accept": "application/json",
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      log: {
        lineNumber: lineNumber,
        columnNumber: columnNumber,
        source: source,
        message: message,
        url: document.location.href,
        browser: navigator.userAgent + '|' + navigator.vendor + '|' + navigator.platform
      }
    }),
  });

  return false;
}
```

What we've done is caught any errors and passed along some of the basic info as a POST message to a non-existent API endpoint on our site (we'll create that soon). We also passed along the CSRF token so the API will accept the call, and made sure to alert the API that it will be receiving a JSON paylod. To test this I added an error that will be thrown 5 seconds after loading the page:

```javascript
// app/javascript/application.js

// ...
window.setTimeout(function() {throw new Error("Whoops!")}, 5000);
```

When the error is thrown, you should be able to see a failed POST call in the network inspection tools of your browser. Next up, creating the controller for logging error messages.

To accomplish that, first we need to create a custom error to raise when the POST message is sent to that controller:

```ruby
# lib/exceptions.rb
module Exceptions
  class JavaScriptError < StandardError; end
end
```

Thank you Rollbar for [your article helping me do that cleanly](https://rollbar.com/guides/ruby/how-to-raise-exceptions-in-ruby). Next up lets create create the routes and the corresponding controller:

```ruby
# config/routes.rb
Rails.application.routes.draw do
  # ...

  post 'logs/js', to: 'logs#js', defaults: { format: :json }
end
```

```ruby
# app/controllers/logs_controller.rb
class LogsController < ApplicationController
  rescue_from Exceptions::JavaScriptError, with: :javascript_error_notification

  # POST /logs/js
  def js
    raise Exceptions::JavaScriptError.new
    head 200
  end

  private
    def javascript_error_notification(exception)
      ExceptionNotifier.notify_exception exception, env: request.env,
        data: js_params
    end

    def js_params
      params.require(:log).permit(:message, :source, :lineNumber, :columnNumber, :url, :browser)
    end
end
```

Alright let's break down what we just saw. First we created the `log/js` route and limited it to the JSON format. When that endpoint is hit we raise a custom error and return a blank page with a success status. The controller catches the custom error and triggers the Exception Notification gem manually. Finally the custom data we passed from the `window.onerror` JavaScript function is caught and filtered using strong params and submitted to the notification call, which explains why we needed the previous `fetch` call's body to be wrapped in `log`.

## Outro

Nice! A custom error monitoring system with the basics, but without the price tag. Also, we can extend this solution in the future creating more custom notification integrations.

A final note I would like to leave here is that this solution could probably use some filtering of error types, especially for the front-end bugs. But this could be a fun next step for you (and me at the time of writing this) to figure out which errors are just noise and need to be filtered.

## Resources and further reading

- [Rails caching](https://guides.rubyonrails.org/caching_with_rails.html)
- [JavaScript error catching](https://blog.sentry.io/2016/01/04/client-javascript-reporting-window-onerror)
- [Rails credentials](https://guides.rubyonrails.org/security.html#custom-credentials)
- [Indie Hackers](https://indiehackers.com)
- [WIP.co](https://wip.co)
