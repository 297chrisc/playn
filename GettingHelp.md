PlayN has a vibrant and helpful community of developers who are happy to help you overcome
obstacles on your way to making kick ass games with PlayN. However, they are also busy people who
are doing this out of the goodness of their hearts. So please read and heed the following
instructions on how to effectively communicate your problem so that it can be answered as quickly as
possible and with as little wasted effort as possible.

## Look for existing discussion of your problem ##

There are three good ways to find out whether someone else has already had your problem and
discussed the solution.

1. Type "playn {some keywords relating to your problem}" into Google. This can be astonishingly
effective.

2. Type "{some keywords relating to your problem}" into Stack Overflow's search box on the `playn`
tag page: http://stackoverflow.com/questions/tagged/playn

3. Search the PlayN Google Group: https://groups.google.com/forum/#!forum/playn

You may not find an answer to your problem, but if you ask about a problem that could easily be
solved by one of the above three steps, you're going to be making a bad first impression on the
PlayN mailing list.

## How to ask about your problem on the mailing list ##

The mailing list is a Google Group, hosted here: https://groups.google.com/forum/#!forum/playn

When you post about your problem, there are certain bits of information you need to provide so that
we even know where to start:

1. Isolate the code that seems to be part of the problem (as much as possible) and include that in
your post.

2. What backend are you seeing the problem on (Java, HTML, Android, iOS, etc.)?

3. Include log output you are seeing for that platform, if any. This is especially important if
this is a build issue.

4. If the output is a visual glitch, send a screenshot.

The best way to post about a problem is to check out the Hello example app, modify it to
demonstrate your problem and then post diffs to the mailing list. This allows anyone on the list to
easily recreate your problem and steer you toward the right solution, or to help isolate a bug in
PlayN if that's the cause of the problem.

The Hello example app can be checked out like so:
```
git clone https://code.google.com/p/playn-samples/
```

then edit the `hello/core/src/main/java/playn/sample/hello/core/HelloGame.java` file, adding
whatever you need to demonstrate the issue.

## Be nice, and be patient ##

Remember that the help you get on the mailing list is coming from people who have day jobs and lots
of other things to do. They are taking time out of their lives to help you because they're nice
people and they want to see more people create the awesome game of their dreams using PlayN. So be
patient if no one gets back to you immediately, and do everything you can to make your question as
easy a possible to answer.
