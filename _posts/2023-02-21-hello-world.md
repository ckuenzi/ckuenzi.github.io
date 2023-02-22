---
#layout: post
title:  "Hello World!"
date:   2023-02-21 20:54:21 +0100
permalink: /hello-world/
feature-img: /assets/images/hello-world.jpg

#header:
#  image: /assets/hello-world.jpg
---
![](/assets/images/hello-world.jpg)

Ever since I first discovered how to print my own name with the Windows command line as a child, I've been drawn to all kinds of programming projects. Many years later I blinked the first LED with an Arduino and the scope of my personal projects quickly expanded to the physical realm. So over time I've accumulated an impressive pile of demos, proof of concepts and mostly finished PCBs. Unfortunately, I've come to the realization that the number of projects I've actually completed to a satisfactory degree is rather small.

#### Why Projects Fail
The cycle of starting 100 projects and finishing none is probably something that a lot of people are familiar with. There are many reasons why the initial enthusiasm oftentimes quickly fizzles out: Lack of time or motivation, unforseen obstacles, unrealstic expectations, fear of failure or crippling perfectionism.
I've fallen victim to all of these things, but generally they happen at a point where the basic mechanism is completed, but the final touches are still missing. In these situations the 80/20 rule often comes to my mind: It says that 80% of the work can be done in 20% of the time. But this also means that the final 20% (the finishing touches) take 80% of the time. This is the reason for this blog currently being called "**The Twenty Percent**".

![One box of mostly half finished projects](/assets/images/box_of_learning_opportunities.PNG)
_One box of mostly half finished projects_

#### Trying to be Better
Just having completed my masters degree puts me in a nice position with lots of free time and motivation to do cool things. I would like to use this opportunity to improve my ability to successfuly complete personal projects. This means especially focussing on the last 20%, which I usually struggle with the most.  

For that reason I'm currently creating this personal blog to document my progress and also get some writing practice. In the beginning the goal is to complete a few simpler projects to hone in on the process, before advancing to more complex topics.  

The post for each project will have three sections:
- **Goal**: Specification of the things I want to achieve with the project. When these things are done, the project ist considered "complete". But some moving of the goal post will probably inevitable...
- **Documentation**: Description of the project itself, including technical details, design decisions and challenges.
- **Takeaway**: Analysis of the process itself, including what went well, what could be improved, and any lessons learned.


## First Project: Creating this Blog
Setting up this site is a perfect opportunity for a first project.

### Goal
I don't have a lot experience with blogs or writeups, so the exact specifications of this blog will probably only be clear once I start using it. But there are some rough goals that I want to achieve:
1. Host the site on GitHub (or some other free alternative)
2. Access the site with my own domain [https://kuenzi.dev/](https://kuenzi.dev/)
3. The ability to write posts in markdown
4. Make it look decent
5. Write a first post (this one)

### Documentation
With a relatively good picture of the final product in mind, I can start working.

#### Choosing a Site Generator
Going with **Jekyll** was the obvious choice, as it is well supported and endorsed by [GitHub pages](https://pages.github.com/) themselves and enables [post](https://jekyllrb.com/docs/posts/) creation with [markdown](https://www.markdowntutorial.com/). It also seems to have a nice trade-off between ease of use and customizability.
I chose to go with a local Jekyll install because the ability so immediately see any changes locally, significantly speeds up the process of fiddling around. Even though I don't have any experience with Ruby, the initial [setup](https://jekyllrb.com/docs/step-by-step/01-setup/) went very smoothely and I had the example blog running in no time.

#### Hosting on GitHub
GitHub even has its own documentation on how to [setup a Jekyll blog](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll) which surprisingly worked without a hitch. I chose to host it as my user page on [ckuenzi.github.io](https://ckuenzi.github.io), as it will serve as a landing page for all things concerning my projects.

#### Using a Custom Domain
To be continued...