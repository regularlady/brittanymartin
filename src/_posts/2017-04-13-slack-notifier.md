---
layout: post
title: How to Add a Slack Notifier with Slack-Notifer and Sidekiq
description: Time to add in a custom incoming webhook in Slack. 
image: assets/images/pic17.jpg
show_tile: false
---
Recently, my boss had the brilliant idea to route the request to a private Slack channel when our Ruby on Rails website processed a customer's contact form. It's ideal for spotting specific website issues and to stay tuned to our patrons interacting with our site.

I came across the excellent slack-notifier gem. I bundled in: 

    gem "slack-notifier"
    gem "json"

Time to add in a custom incoming webhook in Slack. Incoming Webhooks are a simple way to post messages from external sources into Slack. They make use of normal HTTP requests with a JSON payload that includes the message text and some options. Once you have the Slack URL, I added it to our Figaro application.yml as SLACK. 

Next step is to add an initializer for Slack in config/initializers/slack.rb.

    require 'slack-notifier'

    SLACK = Slack::Notifier.new "#{ENV['SLACK']}"

We're already proud Sidekiq users. Processing the Slack message was ideal for a background worker so let's build a SlackNotifierWorker. 

    require 'json'

    class SlackNotifierWorker
      include Sidekiq::Worker
      queue_name = "default"
      sidekiq_options queue: queue_name

      def perform(hash={})
        notification = {
            "username": "csibot",
            "icon_emoji": ":loudspeaker:",
            "fields": [
                {
                    "title": "Organization",
                    "value": "#{hash['org']}"
                },
                {
                    "title": "Path",
                    "value": "#{hash['site_id']}"
                },
                {
                    "title": "Category",
                    "value": "#{hash['category']}"
                },
                {
                    "title": "Notes",
                    "value": "#{(hash['notes'])}"
                }
            ]
        }

        SLACK.ping notification
      end

    end

Remember to set the queue (default since it is not critical), emoji icon (important!) and to utilize Slack's nifty message formatter. 

Our last step is to trigger the SlackNotifierWorker during the flow of a user submitting a contact form. 

    SlackNotifierWorker.perform_async(org: @org, notes: @notes, category: category_string, site_id: site.id)

That's everything. Special thanks to Steven Sloan and Mike Perham for making this so easy to implement. 