---
layout: post
title: How to Deploy Rails with AWS CodeDeploy
description: Step by step instructions for deploying an RoR application on AWS
image: assets/images/pic06.jpg
show_tile: false
---

Now that I'm working at a large non-profit, I'm getting to stretch my DevOps skills a bit farther (with the help of our awesome Ops team). As we're moving our Rails app from version 2 to 4, it was time to see if we could retire the legacy application that we maintained to deploy the old app to production.

Since we're big users of AWS, my team was excited to learn about their relatively new service: [AWS CodeDeploy](https://aws.amazon.com/codedeploy/). CodeDeploy is a free AWS service that efficiently deploys your released code to a "fleet" of EC2 instances while taking care to leave as much of the fleet online as possible. Ship it!

I hunted around AWS's documentation and GitHub for any examples of deploying with Rails but came up empty handed. After a few (OK, many) failed deployments, we came up with a solid workflow.

Here is what we did:

1. Setup an EC2 instance with everything that you need for your production server. In our case, this was Ruby, Passenger, and nginx. You do not want to clone your app via git to the server ahead of time but you will need to know the path of where you want your app to live on the server (for example www/var/...). Make sure you know which users you will use for each process (cloning the code, restarting the processes).
2. Install the [AWS CodeDeploy agent](https://docs.aws.amazon.com/codedeploy/latest/userguide/codedeploy-agent.html) on to the server.
3. Move the EC2 instance to a Production App Group [AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html).
4. In our codebase, we added the following bash scripts to our /script folder. Our full scripts are a bit more complicated (cloning our env vars from a secure s3 bucket) but these will get you off to a solid start. CodeDeploy currently hooks into GitHub only. Luckily, GitHub is what we are using to manage our codebases.
5. Setup a required AWS CodeDeploy **appspec.yml** at the root of your app that references these scripts (see below).

**application/appspec.yml**


    version: 0.0
    os: linux
    files:
      - source: /
        destination:
    permissions:
      - object:
        owner:
        group:
      AfterInstall:
        - location: script/AfterInstall.sh
          runas:
      ApplicationStart:
        - location: script/ApplicationStart.sh
          runas:

**application/script/AfterInstall.sh:**


    #!/bin/bash
    cd /var/www/
    RAILS_ENV=production bundle install --path vendor/bundle
    RAILS_ENV=production bundle exec rake db:migrate
    RAILS_ENV=production bundle exec rake assets:clobber
    RAILS_ENV=production bundle exec rake assets:precompile

**application/script/ApplicationStart.sh:**


    #!/bin/sh
    sudo service nginx restart

Commit these changes and make note of the commit ID. Next, it's time to set up CodeDeploy. From here, AWS has it covered in their [walkthrough](https://docs.aws.amazon.com/codedeploy/latest/userguide/getting-started-codedeploy.html). Wash, rinse and repeat for your Staging setup.

After you have CodeDeploy setup, you will be simply need to know the commit ID, the group you want to deploy to and the frequency that you want your servers deployed in (one at a time, all together or half at a time). CodeDeploy does integrate with our CI service, [TravisCI](https://travis-ci.org/), but we have not setup the integration yet.

CodeDeploy will render a success or a failed message after your deployment completes. If it does fail, CodeDeploy links you to the relevant logs so you can troubleshoot the issue.

AWS CloudTrail automatically logs all of the deployment requests so you have a running log of who and what was deployed. Granted, CodeDeploy still feels new (they update the UI constantly) but I feel confident that the free service will only get better.

I hope more teams will start adopting CodeDeploy once they realize it is free, stable and a great way to shed homegrown tools for deployments.

_Thanks to the Pittsburgh Cultural Trust for giving me a great environment to learn awesome tools like these._
