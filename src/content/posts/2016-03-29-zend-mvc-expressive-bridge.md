---
layout: post
title: "Zend MVC Expressive Bridge"
categories: []
tags: [zend,expressive,mvc]
---

In this blog post, i'd like to promote my zf-mvc-expressive-bridge Module.

## Reason

For a private Project i needed the Zend\InputFilter to validate all of the User Input. While
figuring out, how to integrate zend PluginManager into a Expressive application, i realized it
isn't easy as expected.

After a Short Chat with Matthew, it was clear: I have to create Factories by myself to initialize
the PluginManager as the "default" Factories stay in the Zend\MVC Package.

So i thought there a more Projects, which encounter the same Problem. And the idea of zf-mvc-expressive-bridge was born.

## Realisation

As noted, the Problem are the Factories laying in the Zend\MVC Package. Of course, i could just install this Package
and use only the needed Factories.
But my application is build with Zend Expressive and not Zend MVC so its don't feels right.

Because the needed Factories already exists, this Package only extracts the PluginManager Factories from Zend\MVC
Package. That's it. So all unused classes arn't downloaded.

## Lifetime

In the Chat with Matthew, he noted, that the Problem with the Factories are known and the Solution is on the todo list.
When the Task is done, the zf-mvc-expressive-bridge is deprecated immediately and the use should switch to Factories
provided by the Package (e.g. Zend\InputFilter).

## Conclusion

Hopefully with the zf-mvc-expressive-bridge Package i helped someone and saved some work.