---
layout: post
title: "Zend MVC Expressive Bridge"
categories: []
tags: [zend,expressive,mvc]
---

This blog post is about promoting my zf-mvc-expressive-bridge module.

## Reason

I was working on a private project, where I had to validate all of the User Input with the Zend\InputFilter. While
trying to figure out how to integrate the Zend PluginManager into an Expressive application, I realized it wasn't as
easy as I expected it to be. I had an idea, but that one did not work out.

A short chat with Matthew did help me out here:
To initialize the Plugin Manager I had to create my own Factories - as the "default" Factories are collected in the
Zend\MVC Package. Contrary to this, the PluginManager are distributed over different Packages.

With wanting to help out other projects facing similar problems as mine, the idea of a zf-mvc-expressive-bridge was
born.

## Realisation

As noted above, the problem are the Factories, that are laying in the Zend\MVC Package. Of course, I could have just
installed this Package - with only using the needed Factories. This would have been the easy way.

But as my application is build with Zend Expressive - and not with Zend MVC - it just did not feel right to mix both of
them.

Because the needed Factories already exists, the zf-mvc-expressive-bridge module simply extracts the PluginManager
Factories from the Zend\MVC Package to provide them for further usage.

That's all - this way no unused Classes are downloaded.

## Lifetime

In my chat with Matthew he noted, that the problem concerning the Factories is known and that it is on their to-do-list.
So it's just a matter of time when they'll come up with a solution.
When the task is done, the zf-mvc-expressive-bridge is deprecated immediately and the use should switch to Factories
provided by the Package (e.g. Zend\InputFilter).

## Conclusion

Hopefully, the zf-mvc-expressive-bridge Package helps some of you and saves some extra work.

