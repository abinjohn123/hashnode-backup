# 2022 Wrapped As A Web Development Beginner | Dev Retro 2022

I've been blogging here on hashnode on web development for close to six months. My posts have all been technical, except for one where I shared my [first hacktoberfest experience](https://abinjohn.in/hacktoberfest-learnings). I had been in a cycle of planning and putting off writing one more non-technical post to share my learnings from the past year as a web development learner.

That's when Hashnode introduced 'Dev Retro 2022', a campaign that encouraged developers to reflect on their journey and share their experiences from 2022. It was just the motivation that I wanted!

This post is a log of how I got into web development, my core reasons for taking the development route, and the wonderful events that have transpired in 2022 along the way. I also share a couple of things I learned about the process of learning and development. It's a bit long - probably the longest post I've written - but I'll try my best to make it worth your time!

## Web Development - I Choose You!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671249384733/BsV_dbfCo.gif align="center")

Writing code for fun wasn't something I had done since my school days when I would spend hours in front of my old family computer writing C and C++ programs, trying to build projects for just about any problem I could think of.

I took an involuntary break from it when I got into college as I started to explore [other paths](https://soundcloud.com/abinjohn) that I was curious about. Looking back, I think the image of "the cubicle worker" that people in the software industry were commonly associated with back then did play a role in my drifting away from it.

After a gap of almost five years, I got a first-person view of the world of tech and software after I landed my first job. I realized the cubicle-worker image wasn't entirely true and not everyone hated their job. Although I didn't like the idea of 9-5 much, I enjoyed being in the space and fiddling with tech.

It's funny how the simplest of things can often change the entire trajectory of where you're heading. The first line of code that I wrote at my job was in Java to split a string at a delimiter using the `split()` method. A simple line, yes, but it was special because it was the first time that I wrote code for a project that wasn't being built for myself. It was for my clients, not me. Seeing it run without any errors made me elated beyond words. It also felt great to be on the client call the next day and explain how that line would help solve a minor bug they'd been facing for a few days. It was this single line that made me want to write code again for the sheer thrill that it gave me.

I now needed to choose a language. I didn't want to go back to C or C++ - I'd done enough of that already. Python was too much abstraction for me - I couldn't live without brackets and braces, and Java was simply terrifying. I also wanted something different than a terminal screen for running my code this time; something that users could interact with in a nice way and have fun while using it.

I had started to develop an eye for design and aesthetics right about that time after working as a podcast video editor since the pandemic. A lot of the podcasts had gone virtual because of the lockdowns and most clients wanted a banner or a frame designed for the otherwise plain zoom recordings. That got me thinking - *is there a development path that would help combine logic and design?* I got two options after a bit of googling - Web Development & Mobile Development. I chose web development thinking of the possibility of building tools to help me with my editing business, which already relied heavily on online tools.

## First Steps & The Driving Force

I started off with YouTube videos and playlists on HTML and CSS, but quickly realized I needed something that was more structured and organized. I also got free access to [Pluralsight](https://www.pluralsight.com/) for a weekend and tried one of their web-development crash courses, but that ended up being a bit too paced for me.

My 'dream project' or the major driving force in my initial days was a summary and pay-stub generator application. We used Notion for project management at my editing business and had a few contractors that we were working with. We had to send them a summary of the projects they completed every month and also a pay stub once the payments were processed. Doing them manually was a boring, tedious process and I wanted to automate it.

With the learnings from the crash course and many more YouTube videos, I put together a bare-minimum version of the [application](https://github.com/abinjohn123/hophead-stub-MKIII) that could only generate image files after retrieving data from a Notion database. It sounds classy when I put it that way, but the way it worked was nothing but a mess.

Notion didn't allow API fetches from the front end. So, my app would first generate a cURL query, which I would then paste into Postman, retrieve the result in RAW format, and paste it back into the app for it to work on the data. All the hassle and troubles aside, it helped save significant time and l loved every minute of building it. I knew I wanted to pursue it more.

I turned to Udemy for structured, well-paced learning. I bought Jonas Schmedtmann's [HTML and CSS](https://www.udemy.com/course/design-and-develop-a-killer-website-with-html5-and-css3/) course and started learning the fundamentals from the ground up.

> A neat trick to get insane deals on Udemy is to open the course link without signing in to the platform or on incognito mode. You almost always get the discounted price when not signed in.
> 
> You can sign in just before you hit check out and will get the course for the discounted price.

## Falling In Love With CSS

I had seen CSS get a bad rep online for being complicated and difficult to work with. I also had a hard time grasping selectors, flex, and grid when going through the Pluralsight course. As a result, the first few designs that I built were purely trial and error, and a mix of modern and legacy CSS.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671433311337/PRtYbtO_d.gif align="center")

I started Jonas' course with the same fear, but I was in for a surprise. The way in which he explained the concepts using wonderful examples made grasping them a breeze. I replicated the examples shared in the courses, re-did the designs I'd done before, and sharpened my skills further with online resources like [Flexbox Froggy](https://flexboxfroggy.com/) and [FrontEndMentor](http://frontendmentor.io). Bit by bit, I fell in love with CSS and design.

That is one of the reasons why I love structured courses as well. The ones that are well thought out amp up the difficulty slowly with plenty of examples, making it relatively easy to grasp complex topics.

Today, I'm pretty confident about building basic to moderate layouts. I've taken the time to understand the core concepts and that has been of immense help. Sure, there are certain instances where I struggle, but I do end up figuring them out if I give myself some time.

## On To JavaScript and Codewars

I was so eager to learn JavaScript from the ground up, just like I did with HTML & CSS. And since I enjoyed Jonas' course so much and loved his teaching style, I didn't think twice before purchasing the [JavaScript course](https://www.udemy.com/course/the-complete-javascript-course/) that he is infamous for.

Over the past year, I've been slowly working my way up through the course, taking breaks to build projects with my newfound knowledge after each section is complete. Once the basics were set, I also looked into NodeJS and NPM as I hadn't let go of the dream of building the Notion summary and stub generator.

I also built two other tools to help with the editing business - A notifier app that would send email notifications when a client had a new recording ready, and an app that helped shift timestamps in bulk. (links are given towards the end of the article)

Jonas also introduced a platform called [Codewars](http://codewars.com) in the course. It's a community-built platform where users post questions and other users attempt to solve them. I found it to be a wonderful platform to build problem-solving skills as it had a wide array of questions. It's also possible to see the routes that other developers take to solve questions, and that provides a peek into how others think and approach problems.

I got to learn a lot of cool things from viewing solutions by other devs, like regEx and method chaining. It was disheartening at first to see someone solve a problem in two lines that took you over thirty, but I'm slowly getting there with practice :)

## I Haven't React-ed yet!

I love to go to the depths of what I learn and it is a requirement for me to fully understand the concepts that I work with. I get into a zone of insecurity when I'm not able to. This insecurity is the only reason I didn't pick up React, Angular, or any other library or framework for that matter - a fear that I didn't know *everything* that had to be known about JavaScript before I made the switch. These libraries are an extension of vanilla JavaScript after all.

I've later come to realize that even the most experienced developers often struggle with the simplest of things, and knowing everything is not realistic. I could've switched to ReactJS or AngularJS after I had gotten the JavaScript fundamentals set, but in all honesty, I'm thankful I waited.

I played around with React briefly towards the end of this year. From array destructuring to methods like map and reduce, I could appreciate why the frameworks and libraries existed, and the ease they brought about to the development process.

## Open-Source Contributions

It was a desire to make contributions to the open-source space ever since I first came to know about it. It took some time to get the hang of it and wasn't as easy and smooth as I thought it would be, but it was a fleeting experience to see my contributions get accepted to projects built by people from different parts of the world.

I've explained more about my first open-source experience and hacktoberfest event [in this post](https://abinjohn.in/hacktoberfest-learnings)

## Developing an Online Presence

I knew it was imperative to build an online presence. I was no longer in college, so campus placements were off the charts. Web development wasn't a field I was currently working in either, so nobody would approach me with an offer unless I presented a compelling proof-of-work.

A GitHub profile was a must, but I wanted to take things a bit further. I had tried journaling a bit in 2021 and liked the process of writing. So, I chose Twitter as my medium to document my journey. I committed myself to the #100DaysOfCode challenge and started to share my thoughts and learnings. After a few months of learning JavaScript, I got the idea of starting a technical blog as to share my learnings in a more detailed way.

The insecurity crept in again when it came to blogging. *I'm not proficient enough to be sharing tech content, and I don't have anything new to share*. I found Austin Kleon's [Show Your Work](https://austinkleon.com/show-your-work/) to be quite helpful in tackling this particular insecurity. The book helped me realize that even if the content I put out may not be new or ground-breaking, the perspective and style I bring could be unique and something that somebody somewhere will resonate with. I'm writing for that somebody.

I started Tweeting on [March 29th](https://twitter.com/abin_john98/status/1508701539409723392) and blogging on [July 2nd](https://abinjohn.in/code-faster), and these two have since become an integral part of my development journey. Blogging has helped me become a better teacher as I force myself to come up with examples and analogies to explain technical concepts, and being somewhat active in the Twitter space has introduced me to Indie Hacking, which I'm slowly starting to get obsessed with.

## Tanay Pratap & neoG Camp

I came across an interesting character on YouTube - Tanay Pratap - from a collaboration he did with a YouTuber that I follow. He was an engineer from Microsoft, on the way to building his Ed-Tech startup. He felt different from the other creators I'd seen in the tech and programming space.

For starters, he didn't recommend people starting out with programming dive deep into [content creation](https://www.youtube.com/watch?v=qM-cCsVsDGY). Develop an online presence by sharing your learnings but focus more on building your skills, he said. That would help make more money than going all in with content creation. He also mentioned the credibility aspect. How can a person who's just starting their development journey recommend 'The 5 best practices for developers to follow'? I aligned with a lot of his views and started following his content.

> If you're into long-form content, [this conversation](https://www.youtube.com/watch?v=ua8aWO9Ayzw) between Tanay Pratap and Varun Mayya is GOLD!

That led me to his 6-month community-based Bootcamp called neoGCamp, which took beginner developers and made them industry-ready front-end developers. I've come across many Bootcamps before, both offline and online, but just like Tanay Pratap, his community of web-dev enthusiasts also felt different. The teachers and mentors weren't doing teaching as their primary job, but out of passion. They had day jobs as developers in reputed companies and had years of experience to back them up, so they didn't have any reason to go into teaching except for the love they had for it.

I joined their Discord server (which was free) and immediately loved the space. The members were friendly and warm, and the server itself was managed quite well. The effort to build a healthy, active community was evident.

neoG has a free playlist for beginners called [levelZero](https://neog.camp/level-zero), and then the paid Bootcamp called [levelOne](https://neog.camp/level-one). Students who wish to join levelOne should complete the 15 assignments in levelZero, build a portfolio, and also clear an interview. I was skeptical of the bootcamp at first, to be honest, but I went ahead with it because of two reasons.

*   I loved the teaching style adopted by Tanay. There were no promises of "Learn X in Y days". He wanted students to devote as much time as they could. He approached development and career as a marathon, not a sprint.
    
*   The camp had completed two editions since 2021, and all the graduates spoke highly of the experience they had in the camp and the network that it helped build.
    

The first admission window opened in September 2022. I went through the levelZero content, built the [portfolio](https://abinjohn-portfolio-neog.netlify.app/) as I progressed through the assignments, and then made my submission. I had put in a lot of effort and was quite confident that I would clear the portfolio reviews, but to my shock, I didn't.

I got an email a few days after my submission that it needed revisions. I used Bootstrap in one of the projects out of curiosity to work with it, but that was a red flag for the reviewers. I was asked to switch to plain CSS. The other issue was an API that broke for some reason after I made the submission. I made the revisions and submitted the portfolio again. After an agonizing wait of two weeks, I got the email that I had cleared the reviews. I then scheduled the interview which went quite well, and almost a week after giving my interview, I got a confirmation that I was eligible for levelOne.

I'm currently a levelOne student at neoG. The classes are scheduled to begin in February 2023, and I can't wait to see what it has in store for me.

## Dream Project Breakthrough

Remember the summary and stub generator that I was dying to make? I recently made a major breakthrough in that. All the time spent learning core concepts and building projects finally enabled me to make a working application.

I used NodeJS as the runtime environment, Notion API to fetch the details, the readline-sync library to take any input from the user, and finally, Puppeteer to generate the image and PDF files. It was a long and thrilling ride, but I was super proud that I did it.  
I explained my process in detail as a Twitter thread. [You can read it here](https://twitter.com/abin_john98/status/1595756688300118016)

I'm not stopping there. The next steps are to implement automatic emailing of the generated files, a nice UI (my motivation to learn React \*wink wink\*), and user authentication.

## Projects That I'm Proud Of

I built quite a few projects this year, all with vanilla HTML, CSS, and JavaScript.

1.  [ToDoom](https://github.com/abinjohn123/todoom-app) - A minimalistic ToDo app. I built it to practice DOM manipulation and [Event Delegation](https://abinjohn.in/event-delegation-in-js)
    
2.  [Fee Details Generator](https://github.com/abinjohn123/fee-details-generator) - One half of the "dream project". I haven't pushed the other half yet.
    
3.  [UTrack Chrome Extension](https://github.com/abinjohn123/Utrack-chrome-extension) - A Google Chrome extension that tracks the time spent on YouTube and YouTube Music websites.
    
4.  [Timestamps Converter](https://shift-time.netlify.app/) - An app to timestamp values in bulk.
    
5.  [Riverside Notifier](https://github.com/abinjohn123/Riverside-Notifier) - Get email notifications when a client makes changes to their recordings.
    
6.  [neoG Portfolio](https://abinjohn-portfolio-neog.netlify.app/) - My submission for neoG levelOne qualification. Proud of this because I built everything in under a week.
    

## Learnings

1.  Everybody has a different pace when it comes to learning. Identify the pace that you're comfortable with.
    
2.  Trying out different resources and finding something that worked for me set me up for a better learning experience. Some people might prefer courses, some might prefer reading the documentation, and others prefer going fully ad-hoc with their approach. It's much easier to branch out to other resources once you have a foundation set.
    
3.  Taking breaks after each course section in Udemy to build projects using newly acquired learnings helped me consolidate my understanding of the topics and also realize the ones that I need to revisit. Don't go into tutorial hell. Force yourself to build projects.
    
4.  Build an online presence - Share your learnings and your experiences online on Twitter or LinkedIn. Build credibility and establish yourself.
    

## Plans For 2023

I have two main goals for the next year - Finish the summary and stub generator project, and an indie-hacker project of building a full-stack tool that's beneficial for people and can be monetized.

I'm also all set for neoGCamp levelOne that will begin in February, and looking forward to diving even deeper into web development and meeting my teachers and mentors there.

It was difficult to balance my first corporate job and my editing business because of long, stressful hours. I had to let go of the job even though I had a lovely team and a wonderful manager. So, this year, I'll be trying to find a workplace with a better work-life balance.

That wraps up my Dev Retro 2022 entry. Thank you for taking out the time from your day to go through this lengthy post. It's December 20th as I type this so a heartfelt Merry Christmas to you!

## Resources

1.  [Code With Harry - HTML beginner tutorial](https://www.youtube.com/watch?v=BsDoLVMnmZs)
    
2.  [Flexbox Froggy](https://flexboxfroggy.com/)
    
3.  [Front End Mentor](http://frontendmentor.io)
    
4.  [Codewars](https://www.codewars.com/)
    
5.  [neoG Camp](https://neog.camp/)