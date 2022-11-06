# My First Hacktoberfest Experience

Ever since I started my web development journey a year back, a term that often came up was 'Open Source'. Active open-source contribution was associated with growth as a developer, async work experience, and better opportunities. 

What actually made me interested in open-source was that I'd only worked on personal projects till then. I had also helped friends with small bug fixes, but in most cases, I was just guiding them, and not making any contribution to the project myself. The idea of interacting with and making contributions to projects by people from all around the world was quite exhilarating. I could learn how others approached programming, building and maintaining long-term projects, and working as a team. I wanted to give it a go and started diving into it.

## Getting started with open-source

I faced my own share of struggles as someone starting out in open-source. For starters, I couldn't figure out the workflow of making a contribution. I was under the assumption that I'd just do a `git clone` of the repo that I wanted to work with, make the changes in my own branch, and do a `git push` to push that branch up to the repo, but boy, was I wrong!

A bit of web browsing introduced me to a bunch of new Git & GitHub terms, like fork, fetch, pull, and pull-request, with the last two being entirely different things! I had an understanding by then that making contributions wouldn't be as easy as I thought it would be.

The next problem was the difficulty in finding beginner-friendly projects that I felt at least comfortable with in attempting to make contributions to. The repos of a few public libraries and open-source tools that I'd used were overwhelming with tech stacks I couldn't understand. I spent quite some time browsing through the many platforms and project collectors available to help first-time contributors, but even the ones that were labeled `good first issue` or `first timers only` seemed way out of my skill set.

I had almost given up but, like a miracle, I came across [this repo](https://github.com/danthareja/contribute-to-open-source) by Dan Thareja. A repo that was fully dedicated to newbie contributors and walked them through every step of making a contribution! I've later come to know that there are other similar repos that guide you through the entire process to give you the whole experience, but Dan's was the first repo I found.

I did everything as prescribed in the repo and a few minutes later I created [my first pull request](https://github.com/danthareja/contribute-to-open-source/pull/1564) to a public repo. This was in August 2022, and I was so happy that I was one step closer to contributing to open source.

Luckily enough, my first 'actual' contribution happened just two days later. I came across a repo that had an issue labeled `first-timers-only`, and the task was to just [delete a file](https://github.com/arshadkazmi42/rhof-cli/issues/23) they no longer needed. I faced an issue with the end-of-line sequence because of the way I had configured git. The issue was something that I had not even seen before, and one that I would encounter a lot in the future. A bit of googling and re-configuring git fixed the issue, and I then created the pull request and got that merged. 

## Hacktoberfest 2022

I learned about hacktoberfest while looking into open-source. A lot of people started their open-source journeys during earlier hacktoberfest events. They even gave out swags to participants who made a certain number of contributions during the event.

I loved how hacktoberfest gamified the open-source contribution scene, and for someone interested in Front-End development and slick UI's, their website and login portal were out of the world. I was already excited about participating in the event, but I would be lying if I said that the idea of getting a T-Shirt didn't also play a part in persuading me to participate.

I registered for the event and joined the official discord server - another part of the event that I loved. The server had a dedicated channel for project maintainers to post their projects that were welcoming contributions. Even though the channel was spammed with people posting the same projects every hour or so, seeing that it made project-hunting easy to an extent was a good sight for someone who had a hard-time finding beginner-friendly projects to contribute to when they were getting started with open-source.

Since I'd gotten familiar by now with the open-source workflow, and so I decided that I wouldn't make a contribution just for the sake of it. I wanted to make "actual" contributions for hacktoberfest. So I spent hours going through the projects posted on the discord server, trying out the ones that interested me and finding contribution opportunities that would benefit the projects.

### My Hacktoberfest Contributions

I made a total of six contributions, out of which four were accepted. One project that I contributed to wasn't participating in hacktoberfest, and one pull request didn't get merged to main, making it invalid. 

Interestingly, three of the accepted contributions were made to Google Chrome extension projects. I had built a [chrome extension myself](https://github.com/abinjohn123/Utrack-chrome-extension) as a project and that experience helped me immensely. I was able to easily understand the codebases and make contributions.

Here are all the contributions I made during hacktoberfest 2022

1. [UI Update](https://github.com/immattdavison/NoMoreDomains/pull/96):
  I started off with a UI update for a chrome extension that blocked web domain purchasing websites like Google Domains and GoDaddy. 

2. [Code Refactor](https://github.com/mnosov622/todo-list/pull/51):
  This is a todo-list application. The existing code had code repetitions and what I felt was an inefficient way of loading tasks. I refactored the code and cleaned up the codebase. However, this pull request wasn't merged, and there wasn't any response from the maintainer as well.

3. [UI Update](https://github.com/vinyashegde/shorto_url_shorter/pull/6):
  This was a link shortener chrome extension, and I updated the popup styling to modify the sizing and positioning of a QR code that would be displayed.

4. [Bug Fix](https://github.com/PritamSarbajna/tourism-website/pull/79):
  This was a project that I had my eyes on for a while, even before the event began. I noticed a [bug](https://github.com/PritamSarbajna/tourism-website/issues/76)in the way the nav bar of the page behaved. It took some time to figure out what was causing the problem, but I was able to fix it. Unfortunately, this project wasn't participating in hacktoberfest.

5. [Code Refactor](https://github.com/kartik3805/QuickWiKi/issues/57):
  In this project, I updated how a form was being submitted. The existing code didn't use a form tag and had one listener listening for the click of the submit button, and another one listening for the enter key press. Wrapping up the input element and the button inside a form tag helped condense the code into just a single listener listening for the 'submit' event, which took care of both the submit button click and the enter key press.

6. [New Feature](https://github.com/vinyashegde/shorto_url_shorter/pull/23):
  My final contribution for hacktoberfest was implementing a keyboard shortcut to open up the link shortener chrome extension that I'd contributed to before. My experience with building a chrome extension was quite useful here as well, as I had my own code to refer to when implementing the feature.

## Learnings From Hacktoberfest

My first hacktoberfest turned out to be a great learning experience. I picked up a few things from the exposure to the wide variety of projects that I came across during the event.

1. Proper documentation is crucial. The README.md is what introduces the project, and CONTRIBUTING.md is what guides a contributor. Clarity and detail in both these documents are a must for a great user/contributor experience. 
2. Code consistency is important. A style guide or an editor configuration file, like `.editorconfig` or `.prettierrc` will ensure that all contributors follow the same code style. Something as simple as different tab spacing or having the end-of-line character as CRLF instead of LF can make things go crazy.
3. Fetch/Pull before Push. Things might have changed in the main repo after you forked it and began work. Fetching and pulling in those changes helps to sync your local repo to the most recent state, helping you avoid unnecessary merge issues.
4. Pull-requests descriptions must be highly detailed. Explain the contribution, Include screenshots, and highlight major changes.
5. Not all contributions will be accepted no matter how good you may think they are. It's a hard pill to swallow, but it is what it is.