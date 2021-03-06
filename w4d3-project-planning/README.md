# Project planning and notes on web app architecture

Let's begin with some high-level rules:

## Rule #1

Time is short, so **KISS**!

**K**EEP **I**T **S**TUPID **S**IMPLE

or

**K**EEP **I**T **S**IMPLE, **S**TUPID

(your pick)

## Rule #2

You need to be able to summarize what you want to accomplish in one sentence. Everything else comes after that.

This answers the first question: **WHAT?**

## Rule #3

Don't worry about implementation details at first, but make a list of what you'll need. Use this to break the problem you want to solve into its component parts.

This begins to answer the second question: **HOW?**

While you're doing this, remember **YAGNI**

**Y**ou **A**in't **G**onna **N**eed **I**t

Remove everything that feels optional. Ask yourselves what's the bare minimum for your idea to work. That's your Minimum Viable Product (MVP).


## Alright then, where do we go from here?

Don Burks made an amazing list of things to do next:
https://gist.github.com/donburks/9f47f22cc2fb54ac7396

But in short:

* Take your list of features and make user stories
  + Figure out the data you'll work with
    - Schema
      - Database tables!
      - Relationships!
    - Are you sticking with plain DB queries (`pg`/`knex`) or will you try using an ORM?
      - [Sequelize](http://sequelizejs.com) (recommended)
      - [Bookshelf](http://bookshelfjs.org)
  + Create workflows on how the users will provide that data and interact with it
    - Wireframes!
    - Storyboards!
    - Tools:
      + https://wireframe.cc
      + https://moqups.com/
      + Good ol' whiteboarding
  + It's always better to think from the perspective of the user, not what's easier to implement
  + How to write user stories: https://medium.com/@jonatisokon/a-framework-for-user-stories-bc3dc323eca9


## How to work as a team

### Non-technical aspects

* **GIVE YOURSELF MILESTONES**
  - Set due dates
  - Set development targets
  - Meet them
  - Use [Trello](http://trello.com) to track your user stories

* **SMART Goals** (because you **must** know when something is actually done)
  - **S**pecific
  - **M**easurable
  - **A**chievable
  - **R**elevant
  - **T**ime-based

### Technical aspects

* Use git!
  - `master` should always be _sane_ - in other works, it should **always** contain runnable code
  - Use feature branching
    + A very good tutorial: https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow
    + Git cheatsheet: http://www.ndpsoftware.com/git-cheatsheet.html
  - Use database migrations and seeds
    - Remember: each developer will have their own copy of the database, so migrations will be used to keep the schema in sync, while seeds will provide initial data (e.g admin users)
* Write tests if you can
  - [`mocha`](http://mochajs.org/) is your friend
* Good programmers are lazy
  - Use [Express Generator](http://expressjs.com/en/starter/generator.html) to create an app skeleton, if you're using Express
  - **There's a `npm` package for that**
    + If you're using an external service, look for a package
  - **There's a CSS framework for that**
    + [Bootstrap](http://getbootstrap.com/)
    + [Materialize](http://materializecss.com/)
    + Hundreds of others!

## The three main approaches on writing a web app

### 1. Request-response

This is the way TinyApp works. Every link and form submission results on the page being completely reloaded from scratch. Every single request needs its own EJS template inside `/views`.

The server code receives regular HTTP requests and plain form data, and returns rendered HTML pages based on EJS templates.

By the way, those EJS templates are **server-side**. This means **the server** builds the HTML with the data you provide as `templateVars` and sends the **completed HTML** to the browser.

### 2. Single-page application (SPA)

Tweetr is a good example of this. The HTML is completely static (_not_ EJS) and every action is handled by client-side Javascript code (inside `/public` in Express) with AJAX requests.

The server-side code **only receives and outputs data**, usually in JSON format. This means **every form, button and link _must_ have an attached JS function.**

In other words **the server will NOT use EJS or other server-side templates.** All requests and responses will use JSON.

You may have **static HTML pages** in the `/public` folder, but the data will always be displayed using client-side Javascript to render HTML using JSON data received from the server.

This means more client-side code, but the results are more interactive. [React](https://facebook.github.io/react/), [Ember](https://emberjs.com/) and [Angular](https://angularjs.org/) are examples of client-side frameworks geared towards making single-page applications.

### 3. Mix and match

Some pages are request-response (welcome page and login form, for example) while others have SPA features.

A good example would be an e-commerce application showing a sticky shopping cart panel. Every time an item is chosen, the cart updates without reloading the page. On the other hand, the whole page reloads when the user presses the "order" button on the cart.

**This is usually the quickest approach to write a good, interactive web app for your midterms - or if you have a content-heavy app in general.** It's very common for professional apps to start like this and then migrate to a full SPA architecture.

Some large applications are structured as multiple SPAs with a few request-response actions in between. For example, Facebook and Twitter have a request-response login page, while the main application is a large SPA. A good exercise is using a few of your favourite web apps and trying to figure out which requests reload the whole page.


## Additional stuff for final project

### Show off!

Midterm projects are about exercising what you've learned, gathering feedback from mentors and improving your skills. Final projects, on the other hand, are about impressing potential employers and people in general. So be ambitious and make it look good.

### Don't underestimate UI/UX coding time

Your user interface will certainly be *at least* 70% of your code. **Do not underestimate the time you will spend on it** - even if you use an UI library such as [React-Bootstrap](https://react-bootstrap.github.io/).

### Be lazy!

You don't need to create everything from scratch - we already know you're capable of building apps. Feel free to use libraries and cloud services, as long as they provide something useful for you. This brings us to our next point...

### Explore things, but timebox

If you see a cool library or service you want to try, set aside a **fixed** amount of time to do a test drive - an afternoon should do. If you can do something usefull with it and think it will *save time* in your project, go for it! On the other hand, if the time to get it up and running exceeds your timebox *or* if the gains are negligible, scrap it.

### Stay focused! Do your MVP first!

Always! You need to have a very good idea of what are the main features and what can be left out. Remember this doesn't mean delivering an incomplete app, but a **polished, working app that does what it's supposed to do** with no frills.

### Test integrations

Unit testing (i.e. testing individual functions and methods) is nice, but integration tests are *way* more important. You should definitely test *interface points* - the areas of your app where the user interacts with it and where your client-side interacts with your API. If you're making a SPA, do try to test your API endpoints!

### Don't do a solo project

It can be tempting to go solo, but past experience at Lighthouse Labs tells us this isn't a good idea. Building a polished app in 2 1/2 weeks is no small feat, so having at least two people working together is a must.


## BONUS: Git feature branching cheatsheet

1. Ensure your local `master` is up to date: `git checkout master && git pull origin master`
2. Start your feature branch: `git checkout -b my_feature`
3. _codecodecode_, `git commit`, _codecode_, **goddammit!**, _codecodecode_, `git commit`, YAY!
4. Switch back to `master` and update it again: `git checkout master && git pull origin master`
5. Merge your feature in: `git merge my_feature`
6. Resolve any conflicts (hopefully you don't have any!)
7. Test the updated `master` branch locally
8. `git push origin master`
