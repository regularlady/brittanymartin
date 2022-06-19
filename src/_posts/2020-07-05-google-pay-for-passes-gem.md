---
layout: post
title: Introducing the Google Pay™ for Passes Gem
description: A gem to incorporate Google Pay™ Passes into your Ruby project. 
image: assets/images/pic21.jpg
show_tile: false
---

At the Pittsburgh Cultural Trust, we decided to rollout mobile ticketing. Mobile ticketing is the process whereby customers can order, pay for, obtain and/or validate tickets using mobile phones, without the need of a physical ticket. After a lot of research, we landed on utilizing both Apple Wallet and Google Pay™. 

My assumption going into the project was that the implementation of Google and Apple was going to be similar. Was I wrong! Google wants you to send them the data and they will generate and host the ticket for you. Apple takes the hands off approach of supplying the specification and relying on the application to generate and host the tickets. I was lucky that the [passbook gem](https://github.com/frozon/passbook) existed because it made the Apple implemention pretty straight forward. 

As I turned to implementing Google Pay™ for Passes, I realized that there were not many developers who had done it before. The documentation was difficult to follow (links often linked back to the parent page), you needed a Google Merchant and Google Cloud account and troubleshooting with the logging was tedious. After successfully deploying the implementation for the Cultural Trust, I pondered extracting and opensourcing my work. As it turns out, open sourcing a project that would provide real value to the Ruby community has been a goal of mine. 

Today, I'm introducing the Google Pay™ for Passes gem. After consulting with my friends on the Ruby Blend, the gem is paired with a demo application and I've included the VCR cassette tapes in the repository so developers can see the gem in action. 

* [Gem: regularlady/googlepay](https://github.com/regularlady/googlepay)
* [Demo: regularlady/googlepay-demo](https://github.com/regularlady/googlepay-demo)
* [Demo Site](https://google-pay-demo.herokuapp.com/)

If you're excited about the googlepay gem, please give the gem a star. If you're extremely enthused, please give the gem a try and critique me on the documentation. No observation is too small. I'm looking forward to branching out to other pass types and into payments. 

Note: The googlepay gem is not affiliated with or endorsed by Google.



