---
toc: true
layout: post
keywords: ieee, programming, python
title: My experience running a small programming group for almost a year
# categories: [ieee, programming, python]
hide: false
---

In October last year, I attended a programming group meeting, held by our university IEEE Student Branch. Although at the time the group had been running for at least a year, there were some freshmen university students attending for the first (or second) time. There were slides and algorithms and code written in C++ and I noticed that these freshmen didn’t understand all those things.

So, in the same evening I volunteered to run the little league. A small group with those five younger students who wanted to learn programming outside the university classrooms. For the last couple of years, Python has been my preferable programming language. And it seemed appropriate for the little league.

The plan – we’d spend time introducing the syntax and solving easy puzzles to familiaze with the language and in the best case scenario we would later learn how to do visualizations and how to use an API (to send emails, get the price for the next flight to Amsterdam, print a Star Wars quote, etc).

*Sneak peek into the end of the year’s meetings: Visualizations didn’t work that well, and only two students worked with the API we chose.*

The group meetings started in October and ended in May and we used mainly two tools/platforms: *Jupyter Notebooks (Microsoft Azure Notebooks)* and *codewars.com*.

I didn’t want to install an IDE and explain pip/conda and virtual environments. At least not right at the beginning. And I believe it worked fine as we only installed PyCharm during the last month for debugging the code we wrote while working on an API.

So, Jupyter Notebooks. I would prepare Notebooks with new material and examples and present them during meetings. Sometimes I would publish some exercises and even write assertions after each block of code so they would be able to check their work on their own. And all that inside a Jupyter Notebook. It was great, easy and really useful.

Furthermore, the [Microsoft Azure Notebooks](https://notebooks.azure.com/) platform offered the convenience of cloning a library. I would publish some new notebooks and the rest of the group would just clone my library and get the latest version.

The second platform we used that really panned out was [codewars.com](https://www.codewars.com/). Basically, it’s a place where you can solve puzzles (like CheckIO, CodinGame, HackerRank, etc). The thing that led me to choose this platform was the vast number of puzzles. Because puzzles are created by the community itself, there are thousands of them. At first I would work on some puzzles and then send them to the group. After some time, students suggested which puzzles to solve.

We either solved them during meetings, or solved them at home. We even created a clan. The platform gives you the ability to state to which clan you belong. Our clan was named ieee.xanthi.

That’s how we used most of our time.

I really wanted to show them something cool they could do using Python and I tried that with visualizations. My ambitious plan was to eventually use some data from a data visualization competition on Reddit called [DataViz Battle](https://www.reddit.com/search/?q=%5BBattle%5D%20DataViz%20Battle%20for%20the%20month). Long story short, I used an API to project some data on a map but I guess it was kind of tricky and the group couldn’t follow. I used matplotlib to create some plots but matplotlib didn’t work out for us either.

Closing to the end of the year’s meetings we moved on to APIs. That’s something cool, right? During a meeting I explained what an API is, what endpoints are and tried some of them through the [rapidAPI](https://rapidapi.com/) platform. After that, I figured that we’d need a simple, well documented and clean API to use. Surprisingly, the [API on the codewars platform](https://dev.codewars.com/) seemed like a really good choice! It’s simple, it’s clean and you can build upon it! Moreover, to build upon it, combine data from different requests and make pie charts out of the data we’d get, we used much of the things we’d seen so far.

For example, I had them write a function that would get the names of two users, request through the API the puzzles solved by each one of them and return the names of those that were yet unsolved by each user.

Another one was to get the difficulty level for each one of a user’s solved puzzles and draw a pie chart using them.

In retrospect, I’d do some things differently. Mainly, I’d try a pseudocode-centric and not Python-centric approach. During those months I sensed that what freshmen students lack is algorithmic thinking. If it’s hard to figure out a basic algorithm that tests if a number is prime or not, then we should focus on that first and on code implementation later.

[Here](https://notebooks.azure.com/xen0f0n/projects/python-ieee) is the link for my library on the Microsoft Azure Notebooks platform.

All in all, even though only two out of the five members of the group really followed along until the very end...

![Alt text](https://media1.tenor.com/images/c4bb9246ba107ea847f4bb66b6e0a99c/tenor.gif)