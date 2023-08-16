---
#layout: post
title:  "Hello World!"
date:   2023-02-23 12:54:21 +0100
permalink: /hello-world/
feature-img: /assets/images/hello-world.jpg

#header:
#  image: /assets/hello-world.jpg
---
![](/assets/images/hello-world.jpg)

Ever since I first discovered how to print my own name with the Windows command line as a child, I've been drawn to all kinds of programming projects. Many years later I blinked the first LED with an Arduino and the scope of my personal projects quickly expanded to the physical realm. So over time and many projects, I've accumulated an impressive pile of demos, proof of concepts, and mostly finished PCBs. Unfortunately, I've come to the realization that the number of projects I've actually completed to a satisfactory degree is rather small.

#### Why projects fail
The cycle of starting 100 projects and finishing none is probably something that a lot of people are familiar with. There are many reasons why the initial enthusiasm oftentimes quickly fizzles out: Lack of time or motivation, unforeseen obstacles, unrealistic expectations, fear of failure, or crippling perfectionism.
I've fallen victim to all of these things, but generally, they happen at a point where the basic mechanism is completed, but the final touches are still missing. In these situations the 80/20 rule often comes to mind: It says that 80% of the work can be done within 20% of the time. But this also means that the final 20% (the finishing touches) take 80% of the time. This is the reason for this blog currently being called "**The Twenty Percent**".

![One box of mostly half-finished projects](/assets/images/box_of_learning_opportunities.PNG)
_Box of mostly half-finished projects_

#### Trying to be better
Just having completed my master's degree puts me in a comfortable position with lots of free time and motivation to do cool things. I would like to use this opportunity to improve my ability to successfully complete personal projects. This especially means focusing on completing the elusive last 20% of the project which takes so much effort to do.

For that reason, I'm creating this personal blog to document my progress and also get some writing practice. In the beginning, the goal is to complete a few simpler projects to hone in on the process, before advancing to more complex topics later on.   

The post for each project will have three sections:
- **Goal**: Specification of the things I want to achieve with the project. When these things are done, the project is considered "complete". But some moving of the goalpost will probably be inevitable...
- **Documentation**: Description of the project itself, including technical details, design decisions, and challenges.
- **Takeaway**: Analysis of the process itself, including what went well, what could be improved, and any lessons learned.

![One box of mostly half finished projects](/assets/images/box_of_prints.png)
_Box of 3D printed learning opportunities_


## First Project: Creating this Blog
Setting up this site is a perfect opportunity for a first project.

### Goal
I don't have a lot of experience with blogs or writeups, so the exact specifications of this blog will probably only be clear once I start using it. But there are some rough goals that I want to achieve:
1. Host the site on GitHub (or some other free alternative)
2. Access the site with my own domain [https://kuenzi.dev/](https://kuenzi.dev/)
3. The ability to write posts in markdown
4. Make it look decent
5. Write the first post (this one)

### Documentation
With a relatively good picture of the final product in mind, I can start working on the project.

#### Choosing a site generator
Going with **Jekyll** was the obvious choice, as it is well supported and endorsed by [GitHub pages](https://pages.github.com/) themselves and enables [post](https://jekyllrb.com/docs/posts/) creation with [markdown](https://www.markdowntutorial.com/). It also seems to have a nice trade-off between ease of use and customizability.
I chose to go with a local Jekyll install because the ability to immediately see any changes locally, significantly speeds up the process of tinkering around. Even though I don't have any experience with Ruby, the initial [setup](https://jekyllrb.com/docs/step-by-step/01-setup/) went very smoothly and I had the example blog running in no time.

#### Hosting on GitHub
GitHub even has its own documentation on how to [setup a Jekyll blog](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-github-pages-and-jekyll) which surprisingly worked without a hitch. I chose to host it as my user page on [ckuenzi.github.io](https://ckuenzi.github.io), as it will serve as a landing page for all things concerning my projects.

#### Using a custom domain
Free hosting on GitHub is nice and all, but using a custom domain elevates it to the next level. Luckily this can be done rather easily by [changing some DNS records](https://gist.github.com/plembo/84f80c920bb5ac6f19e53fe6f8db1ff7). The hardest part was being patient for an hour until the DNS changes were active and the certificate was generated.

#### Making it look pretty
The default [minima](https://github.com/jekyll/minima) Jekyll theme already looks nice, but I decided to go with the dark version of the very popular [minimal-mistakes](https://github.com/mmistakes/minimal-mistakes) theme because it looks a bit more modern and seems to have a lot of nifty features. The nice thing about a program like Jeykll is that the actual posts are written completely independently of the theme, which can easily be changed later.  
But even with a complete theme, there are a lot of screws to turn and styles to try. This is a part of the project that I spent a lot of time on, especially because of the sophistication of this particular theme. There are still some things that I would like to improve such as including header images and a side-bar.  
But no matter the theme, this site will look a bit barren without the actual content.

#### Writing the first post
With all the technical aspects working, this is the point where I reached my **last 20%** for this project. Writing this post took way longer than expected, taking a lot of perseverance and motivation. And with the hype surrounding ChatGPT at the moment, it required a lot of restraint to not outsource all the work to an LLM.
There are many things that I'm not yet sure of, such as the writing style/tone and the actual content for this blog. But these things will hopefully become more apparent with time and additional content.

### Takeaway
When this subsection is written, I successfully completed my first Project. All the technical stuff was pleasantly straightforward without any unforeseen difficulties. It was also interesting to actively observe the dip in motivation once I had to start writing, which easily took 80% of my effort. It will also be interesting how that plays out with future projects that are not _this_ meta and need writing in addition to the rest of the challenges.  
For the future, it would be lovely to not procrastinate as much as I did once the hard part inevitably showed up. But carefully observing the dip in motivation and the experience of having pushed through will hopefully help with that.
In the end, I'm satisfied with the outcome of this project, as all of the goals were satisfied and I now have a product that can be easily used for future posts.

<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-JCFYDY59EG"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-JCFYDY59EG');
</script>