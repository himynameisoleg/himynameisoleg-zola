---
tags:
  - blog
title: Replatforming from Gatsby to Zola!
author:
  - Oleg Perchyk
date: 2024-04-01
---
# Intro
So I've had my fair share of personal websites and blogs. I have built them on  stacks ranging from the most basic HTML and CSS, to hosted frameworks like Wordpress and Laravel, to the more modern single page applications built in Vue and React. For a simple content blog I think you can't go wrong with a Static Site Generator though. These days I am almost exclusively writing everything in [Obsidian](https://obsidian.md). Which is great because its all in standard markdown format. This allows for a really neat and easy content publishing workflow.

# The Workflow
 Since around 2019 I have used [Gatsby](https://gatsbyjs.com) as my static site generator. Its plugin system makes it super feature extensible. It uses React under the hood which makes components easy to write and has tons of community support. Once I had a Gatsby site styled and running, publishing blog posts is fairly trivial:
 
 1. Draft up a new post in markdown and tag it appropriately
 2. Copy the markdown file over to the Gatsby site **content** folder 
 3. Push the changes to main branch on GitHub
 4. Wait as the [Netlify](https://netlify.com) CI/CD robots work their magic to build and push my updated blog site with the shiny new post to the open internet

Pretty neat right?

Well, naturally time moves on. Technologies mature, one tech company acquires another, dependencies become old, vulnerable and deprecated and sooner or later the dependabot alerts start pouring in. So here's what prompted the change in the first place.

# Why Change?
After finding a few spare hours I decided to address the alerts and update some my dependencies.  I spent several hours debugging my Gatsby site after doing some recommended npm package updates. My UI class library [Bulma](https://bulma.io) was not being loaded by my sass-loader module. (I later learned that they migrated to dart-sass so I guess the fix should have been a pretty easy). Nonetheless, this prompted me to rethink my entire static site generator stack and got me curious about some other options. Why have these unnecessarily complex dependency chains? At that point I almost too hesitant to even touch my package.json file. That's just silly.

So after shopping around a bit I found a simple, dependency-less static site generator called [Zola](https://www.getzola.org). The lack of dependencies sounded very attractive after all the headaches trying to update my Gatsby modules. I wanted to give Zola a try and see what tradeoffs I would need to make coming form a React-based framework to this Rust-based generator. 

> tldr; not a whole lot.

# The Process
After a quick run through the Zola [getting-started](https://www.getzola.org/documentation/getting-started/overview/) docs, I had my "Hello World!" app running and I was ready to start dragging components from my Gatsby site into the Zola site. 

What I immediately loved was how everything for handling markdown and styles was already baked in. I didn't need to install any special plugins to get markdown to work. I didn't need to write complex GraphQL queries to populate my posts  as I did with Gatsby. Images worked out of the box, pages and posts showed up right away so long as I followed the prescribed directory structure.

I pulled the content markdown files and scss stylesheets from my old site into the appropriate folders of the Zola site, updated a few tags, and it **just worked**! 

Now  going from a component based library like React back to plain HTML required a few changes, but was fairly simple. It was mostly a process of renaming custom tags back to **div** and changing some looping constructs from JavaScript to the mustache format. Zola uses the [Tera](https://keats.github.io/tera/) templating engine which is very similar to the template syntax I have used in Python Flask projects at university so it was fairly simple to lay everything out and get the site looking almost exactly like my old one. With a few caveats.  

# Challenges
This replatform only took me two short sittings from start to deploy, but it definitely had its own quirks and challenges. Here are a few issues I encountered during the migration:

## Image paths
I had a few issues with images not showing up from both my markdown files and my main images folder. I learned that during the site generation, the images are fetched from the **static** directory and placed into the **public** directory root. So when referencing images from a markdown I had to do it relative to the public folder not relative to my source directory structure. 

So for images stores in **static/images/** the correct relative paths would need to be something like:

e.g. for markdown
``` markdown
content/blog/myPost.md

![myImage](../../images/blog/myImage.png)
```

eg. for HTML and CSS
```css
/* sass/myStyle.sass */

.myPhoto {
  background: url("images/site/image.png");
}

```

I learned to always refer to the layout of the public dir rather than my source whenever images had issues. 

## Mobile responsive scaling
After deploying I noticed that all of the mobile reactive styling was not being applied. After some digging I found this [Mozilla article](https://developer.mozilla.org/en-US/docs/Web/CSS/Viewport_concepts) explaining viewports. By default Zola does not set any scaling options, so the mobile-responsive scaling was not working. Simply adding the meta tag below solved this.

```HTML 
<meta name="viewport" content="width=device-width, initial-scale=1" />
```

## Top 5 
One feature I wasn't able to carry over from my old site was my front page "Most Recent" posts. In Gatsby you can write a GraphQL query to fetch the top 5 posts by date and wrap them in a looping construct to be displayed dynamically. I havn't figured this out with Zola yet, so if anyone has suggestions lets get in touch!

Eventually I want to be able to display the top 5 most recent posts dynamically, but for now I'll just use static HTML.

## Deploying to Netlify
I think the documentation might be a little out of date on this one. When I first deployed following the docs instructions my site would hang for 20 minutes during the build step, eventually hitting Netlify's execution time limit. I eventually followed [another tutorial](https://dev.to/davidedelpapa/zola-tutorial-how-to-use-zola-the-rust-based-static-site-generator-for-your-next-small-project-and-deploy-it-on-netlify-375n) which recommended adding configs to a **netlify.toml** file. This resolved my issue immediately and the builds were blazing fast 🔥.

Here's my netlify.toml:

```toml
[build]
publish = "public"
command = "zola build"

[build.environment]
ZOLA_VERSION = "0.18.0"
```
## Documentation and Plugins
Zola's documentation isn't nearly as thorough or robust as Gatsby. The community is pretty tiny by comparison. I searched YouTube for some tutorials and only found a handful of videos... and half of them were about a completely different static site generators like Hugo.

## Vanilla JavaScript
Well the elephant in the room is "do you miss React"? And honestly, not really. I only ever had a two simple JavaScript functions:

1. one to handle opening and closing the hamburger menu
2. and a second on to dynamically update the year in my footer

Why do I need a massive JS bundle to do something so simple. Yes components are really nice and React has some super powerful features, but it was honestly really fun going back to VanillaJS and manipulating DOM elements directly. 

For me it was especially nostalgic. It reminded me of the summer in 2016 that I spent on my grandparents' farm in the village in Ukraine 🇺🇦. I was just a budding web developer and I brought Jon Duckett's famous [JavaScript & jQuery](https://amzn.to/3VDEepi) book with me. I read it cover to cover and learned JavaScript that summer. it was fun to go back to those basics.

For those curious, here's all the JavaScript I needed:

```javascript
 // hamburger toggle responsive menu
  document.addEventListener('DOMContentLoaded', () => {
    const burger = document.querySelector('.navbar-burger');
    const menu = document.querySelector('.navbar-menu');

    burger.addEventListener('click', () => {
      burger.classList.toggle('is-active');
      menu.classList.toggle('is-active');
    });

    // Get the current year and set it in the footer
    const year = new Date().getFullYear();
    document.getElementById('dynamic-year').textContent = `© ${year} himynameisoleg`;
  });
```

# Outro 
Well thats all it really took. It was a really fun project and I highly encourage anyone to try out some static site generators like Gatsby, Zola, Hugo... to name a few. I think every major front-end framework and modern language has its own flavor these days.

Looking ahead, I am thinking for the next big replatform I'll try a slightly different approach. I want to build  out on  traditional hosted solution like Ruby on Rails, Laravel or Rocket and try out CloudFlare [caching](https://developers.cloudflare.com/cache/).

Till then, L8er!
 



