+++
author = "Periklis Gkolias"
categories = ["new_codebase", "legacy_code", "code_reading", "open_source", "oss_contributions"]
date = 2018-03-28
description = ""
draft = false
slug = "how-to-get-familiar-with-a-new-codebase"
tags = ["new_codebase", "legacy_code", "code_reading", "open_source", "oss_contributions"]
title = "How to get familiar with a new codebase"

+++

Most of us, quite often, are in need to familiarize with a new code base. You might be an avid open source guy/girl who wants to contribute to another project,  or you just started a new job, and you want to familiarize with the platform that will be a big part of your life, during the next months/years...or just for fun, because who's watching comedies nowadays?

![c47f2b77e60700bcd8e283155c00677b5acc6a22dbb6951d032002f71058ef77](/img/2018/03/1_Js4O-RaZbAfFa-UIw_1LJw.jpeg)

Familiarizing with a new code base is a keystone skill, every software engineer should acquire on this beautiful planet.

Quite a few people have shared their techniques over the years. But I believe that repetition and collaboration are absolutely essential when it comes to master a skill.

The approach below, focuses on solving the problem on your own and not "relying" on another human being for mentoring, like an architect in your company.

# RTFM first
Read any documentation the software might be associated with, first thing. This might be things like wiki pages built for the project or documentation build from code comments like doxy or Javadoc.

If the software is too large then you might prefer focusing on specific areas.  For example, If you want to get familiar with the entire Facebook platform,  you could start by reading about how news feed is being populated or how the data are structured in the database layer. Flow diagrams, class diagrams etc that were used as requirements reference are always handy.

# Build it, break it, shake it(no pun intended [Savage Garden](https://www.youtube.com/watch?v=tJY6xGm_6f0))

The natural tendency, after you have acquired the some basic knowledge about the system you are examining, will be to build it.  If it cannot be built, maybe it's not worth reading it.

But, if for any reason you dont care, understanding how the build is done (build process) is crucial for your end target. Knowing the build process is important, because you get to know the dependencies and how the built code is deployed. And knowing that dependencies means, you know what the f*** might have broken your system.

After you build it, start using it as an end-user would do,  but don't just follow the happy path. Try to break the interface and see how the system reacts.

At the same time, observe the logs produced. This might reveal some sequence in the process, and ideally, it will point you to an entry point to start looking at the code.

For example, if you want to examine the login process of the system, log in as a user would do. But maybe try and see how it reacts to a couple of 'form attacks' like [SQL injection](https://www.owasp.org/index.php/SQL_Injection) and [XSS](https://www.owasp.org/index.php/Cross-site_Scripting_(XSS)).

If the logs imply that the handler `handle_user_login` is called as part of the interaction, this is totally a good entry point to start reading the logic.

# Read the automation tests
When I say automation tests, I don't only mean unit tests but other types too, like behavioral tests([Cucumber anyone?](https://github.com/cucumber/cucumber)). Here a few reasons for that:
* The tests, given they are written correctly, provide a great demonstration of how the app is expected to work, in terms of flow. For example:
> In order to get the user logged in, I have to involve those 5 functions, that are mocked in the test, so let's check them more deeply.
* The unit tests are a great source of documentation.If you read the unit test(s) that emulate the login success of a login you have a good example of the involved functions and their return values. What does function `create_hashed_password` return? Is it a boolean or the hash itself? You can now verify.
* The behavioral tests are a living requirements capture mechanism. If you are not familiar with the concept of behavioral tests, [this](http://docs.behat.org/en/v2.5/guides/1.gherkin.html) is a great article to have a quick but good grasp.

# Start reading and drawing things
Now that you know where to start, start reading the involved pieces of code. In the example we have discussed so far, try to understand how the login handler parses the request. How the parsed password is converted to a hashed version and how it is compared to the one that we have stored in our database.

Don't be afraid to dive deeper and get a better grasp on the low-level implementation. But, make sure you discipline yourself and you know when to stop. Otherwise, you might end up planning how to make flushing data to the database 2 pico-seconds faster(unless thats your end target of course).

Try to understand, why the developer implemented a specific piece of logic in a certain way and how would you do it. Start drawing logic diagrams that will help you visualize the flow.

Note keeping, as well, is more than desirable. Especially, if it is in terms of inline comments, which will help revisit the code later, less painfully this time. In such case, my preference is to prefix them with my initials, to filter them easily later.

And what's the best way to verify your understanding? Modify the code and write unit tests. If the code you added, is invoked, you are certain you are on the correct track. Similarly with the unit tests. You cannot write non-trivial tests if you don't have a decent understanding of the logic of the feature you examine.

# Trace the full process in detail
Now that you know, that you are focusing on the correct piece of code, you may move further and into more details. One good approach would be to add breakpoints throughout the code invoked. Maybe start the debugger and start verifying what is going on, in terms of variables and values.

Are any of those, initialized in a different place comparing to where you expect them to? Do they have the correct format or are being [coerced](https://dev.to/promhize/what-you-need-to-know-about-javascripts-implicit-coercion-e23) somehow later? What does this variable with the ugly unclear name supposed to do?

What preparation is being made from functions called before the handler under examination? What are some assumptions, being made for each call, and how we are verifying them?

#  Rinse and repeat
Now that you have a good understanding of what is going on for a certain slice of the functionality, you can pick another one. Learning is never-ending and always exciting.

# (Optional) Try adding a new feature or solve a bug
This is mostly applicable to open source projects but not forbidden with 'plain' enterprise software. Once you feel confident with the software you are examining(or at least with the area you are mostly focused on), try adding some piece of functionality, whether this is a bug fix or a small new feature. 

This will increase your confidence regarding the system, for sure, and is always rewarding adding new code to an existing system.

# Books to read
* [Working effectively with legacy code](https://www.amazon.co.uk/gp/product/0131177052/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=0131177052&linkCode=as2&tag=bitperi-21&linkId=98aa847bab0a8691243724c15d567a9a)
* [Code reading](https://www.amazon.co.uk/gp/product/0201799405/ref=as_li_tl?ie=UTF8&camp=1634&creative=6738&creativeASIN=0201799405&linkCode=as2&tag=bitperi-21&linkId=4ae3be10ab8bd529e84cf6b41567b5fa)

# Summary
Thank for reaching the end of this article. Please let me know if you agree/disagree with any of the steps above and as implied in the introduction, feel free to add your own methodology. Do you have any quality books to suggest, on that area?

Below is a summary of the steps I usually follow to familiarise with a new codebase:
* Read the documentation available, the first thing
* Build the software and use it as an end user would do
* Read the automation tests to get a first dive
* Read the actual business logic and make diagrams out of your understanding
* Trace the full process of each slice in detail
* Move to another slice of functionality, once confident
* Add some value to the system, by contributing to it with a bug fix or a new small feature