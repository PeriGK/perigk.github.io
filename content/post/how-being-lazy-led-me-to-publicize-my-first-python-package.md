+++
author = "Periklis Gkolias"
date = 2018-05-08
description = ""
draft = false
slug = "how-being-lazy-led-me-to-publicize-my-first-python-package"
title = "How being lazy, led me to publicize my first python package"
tags = ["pypi", "python3", "package", "python"]
+++

# How being lazy, led me to publicize my first python package

There is a common saying, in the software industry, which goes like:

> I want to hire a lazy developer because he will find the way to deal with
> difficult problems in the shortest and easiest manner.

I wasn’t there the first time this was told, but rumors say that these words
belong to **Bill Gates**.

I don’t know if Mr Gates would ever hire me(wink wink) but I have to admit that
having that kind of laziness, has led me, from time to time, to create
interesting and time saving libraries/scripts/workarounds.

![](https://cdn-images-1.medium.com/max/800/1*nMTVWvIzsKtSC2qntRAlqA.jpeg)
<span class="figcaption_hack">Thanks Larry :)</span>

As you can see my profile, I’m a big fan of python. I love the great combination
of elegance, power, and extendability the language provides at no discounts. I
have noticed though, a boilerplate-code pattern in quite a few pieces I have
read, in that language.

This pattern was that when entering a function 1) we are logging this event,
usually for debugging purposes and 2) quite a few times we need to handle a
potentially raised exception sometimes in a trivial way like below.

    import os

    try:

        os.remove(a_file)

    except:

        print(‘exception raised’)

And as with all the* ‘lazy’* programmers on this beautiful planet, I am not a
fan of boilerplate code.

That’s how I forced myself(are lazy people forcing themselves?) to find a
*convenient* solution. Which, by the way, has a lazy name too: **catslog, **as a
few cats have inspired many memes for laziness.

#### The technical stuff

The solution involves the fantastic feature of decorators. I like it so much,
that instead of movies, I am reading code with decorators(and various other
python features)

![1_9dgT7Mg-YLMGW7b1MpIrAA](/img/2018/05/1_9dgT7Mg-YLMGW7b1MpIrAA.jpeg)

<span class="figcaption_hack">Sorry, that was a very bad joke</span>

In a more serious note, I will not dive into decorators here, as there is
already a vast collection of great articles around, but I highly recommend you
to read about them, if you are no familiar. Like this classic
[primer](http://simeonfranklin.com/blog/2012/jul/1/python-decorators-in-12-steps/).

But if you are…lazy(no pun intended) and don’t want to spend time on that, it is
sufficient to know that:

> A decorator, is a function, which extends another function(let’s call it foo),
> and replaces the original foo reference with the extended one.

A bit like the below example:

    def foo():
        # Various operations going on

    foo = my_decorator(foo)

Back to my package, the core logic can be seen below:

    from functools import wraps

    def catslog(f):
        """Given a function f, run it, log the execution and
        handle any potential exceptions"""
        
    (f)
        def wrapped(*args, **kwargs):
            """The extended version of the function f"""
            try:
                func_name = f.__name__
                print('Executing function {} with args: {} and kwargs: {}'
                      .format(func_name, args, kwargs))
                f(*args, **kwargs)
            except Exception as e:
                print("""An error occurred while executing function "{}".
                The error is {}""".format(func_name, str(e)))
        return wrapped

That all. A really lazy solution, in order to move over my laziness.

Long story short, we create the function which creates another function which
extends our original(decorators in one line, as explained above).

The code can be used as:



#### Don’t you have others to do in your life, Periklis?

But why publish it, you ask? First of all, the lazy factor(again? #ohgodwhy); I
can use it to any dev environment I might work, without a hustle. And of course,
it is extremely rewarding giving back to a community that is so helpful,
supporting and full of top-notch engineers.

#### That’s all folks

Thank you for reaching the darkest depths of this article. I hope you enjoyed
reading it, as much as I enjoyed the process of creating the package and writing
this article.

I am pretty sure that you have a time-saving code snippet which you most
probably copy-paste around) that is worth sharing and I highly encourage you to
publicize it, even if it is not in python.

More details about the project can be found at the github
[page](https://github.com/PeriGK/catslog) of the project. As always, feel free
to ask anything in the comments section.

* [Python](https://medium.com/tag/python?source=post)
* [Pypi](https://medium.com/tag/pypi?source=post)
* [Python Packages](https://medium.com/tag/python-packages?source=post)
* [Open Source](https://medium.com/tag/open-source?source=post)

By clapping more or less, you can signal to us which stories really stand out.

### [Periklis Gkolias](https://medium.com/@periklisgkolias)

In love with Python, but I admire all the stacks that offer solutions without
testing my patience. Avid productivityist, great-food worshipper,
always-smiling.
