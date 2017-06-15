Speaker Notes
=============

## 00-introduction

Hey thanks <previos speaker>! Hello everybody! There are a lot of new faces around EverFi, so I'll introduce myself a bit. I'm Chris Barnes. I'm a front end developer here, so I deal mostly with HTML/CSS and JavaScript. Although I know many programming languages including Ruby, SQL, D, C#, Java. Anyway, we're a little unique on the front end team in that we wander around just about all of the apps and build the UI. I'm here to talk about sharing chunks of HTML between all of our apps using Railsifi. I've built nearly all of what I'm about to talk about, and we're just starting to use it as a few new apps come online. The older apps will have to migrate to a new HTML structure if they ever end up using this stuff. So anyway let's get into it...

## 01-the-problem

First, I'd like to enumerate the problem that cropped up. We were building Next, Engage, and Adminifi. All of these apps pretty much look the same. They've all got a header, and a footer and an admin section that has a sidebar with links in it. At the time, all of the HTML in the layouts was copy and pasted around because HTML is not even really code right? HTML is not really a language despite the initialism.

As each of the apps was being built, all of the HTML started to drift apart as copy and pasted text tends to do. It's fine to have repetition in code as long as the code isn't representing the same concept, but clearly the common components that built up the apps were in fact the same concept. Like a header and a footer along with the CSS that styles them.  Now three apps is one thing, but we're going to be building a lot more of these little modular apps going forward, so we've got to do something about this.

Here's a concrete, albeit diminutive, example of how the header drifted apart over time and how that makes it harder to style with CSS.

## 02-next

Here's the header in Next. Not complicated.

## 03-engage

Now here's the same header in Engage. Someone had the very good idea to use the semantically correct `<header>` element, which is nice.

## 04-adminifi

Finally, here's the same header in Adminifi. Again, you can see the minor ways even a simple header can drift. So let's say I want to style the header a bit on all the apps. I'd have to write some styles in the everfi_material gem to get it shared amongst all the apps. So let's make the header blue.

## 05-css-target-1

Will this work? Nope. It sure won't. This will only work in Next.

## 06-css-target-2

Well at least all of the apps used the `<header>` element to describe their headers. Oh wait, Next didn't. So this selector won't work either.

## 07-css-target-3

There we go! Using the class and the id will cover all of them. While this is a fairly simple example, hopefully it illustrates a need for a common core of HTML and CSS across all of the apps. Particularly as we build a bunch more apps going forward.

Otherwise we'll end up writing CSS like this:

## 08-frustrated

## 09-a-better-way

Ok, so we've enumerated the problem. Let's get at one of many possible solutions.

## 10-railsifi-shared

Railsifi contains a lot of shared view partials, layouts and a suite of helper functions to generate HTML with the proper classes. We want this to be flexible too. If the apps need some special treatment, that's fine, and we should be able to be flexible enough to accomodate that, while still putting some contstraints on, such that we can style everything sanely.

## 11-layouts

First off, Railsifi contains two layouts. A default and an admin layout. Each layout has a bunch of slots which can be filled using `content_for`. So we've got a common overall structure, yet each app can put whatever it wants into each of those blocks.

## 12-layouts-how

Here's how you can render the admin layout. You can feed it blocks for :title, :stylesheets, :header, :navigation, :footer and :scripts. Each of those has a default, so we may not even have to include anything in certain cases.

## 13-partials

Now each block that's also a component of somekind, like the header and sidebar also have a helper method associated with it in the module Railsifi::Helpers.

## 14-partials-how

Here's an example lifted out of LMS Compliance. This helper method is sort of like the one's built in to Rails, like `link_to`, and can take arbitrary HTML, and assign arbitrary HTML attributes on each of the list items. The hash passed to `n.item` will specify the HTML attributes for the `<li>` element, and a block that will be rendered inside of the `<li>` element. This code will render to this:

## 15-navigation-screenshot

## 16-other-helpers

Right now at least, there are a few other helpers similar to the navigation helper we just looked at. There are helpers for the footer, the header (which looks really similar to the navigation helper), the icon fonts, navigation, and the EverFi Logo. That'll pretty much cover the basic idea around all of the partials, layouts and helpers, but we're not quite done yet. Here's a bonus.

## 17-module-classes

The layouts include a few super useful classes on the body. If the controller from which the view is served is nested deeply in a module, it'll include the name of each module, the name of the controller, and the name of the action.  As such you can create a CSS file that will be psuedo-scoped to any of those levels.

## 18-module-classes-example

Here's a quick example of some CSS which is which is jailed to the admin login screen. Admin is the module, sessions is the controller and new is the action. You can provide CSS that is scoped to everything in the admin module, everything in the sessions controller or only that specific action. This hierarchy should allow us to organize the CSS in a more sane way, by separating each distinct concept into it's own file and not letting the details of that concept leak out accross the app and break other things in other locations.  Alright! That does it, you're all experts in Railsifi now. I'd suggest you go have a look at the source on GitHub to get a sense for how things fit together. There's some really interesting uses for the capture helper in there.

That's all up on Github and you can have a look at your leisure.

So, that's all I've got...

## 99-questions

Thanks for listening! Does anyone have any questions?
