---
title: Structuring iOS Apps
layout: post
published: false
---

When building iOS apps, there have been some patterns that have been helpful.

I prefer my ViewControllers to only control their views, and no business logic. They should facilitate the animations and moving around of the views.

Any 'business logic' should go in dedicated business logic controllers.

This is handy when multiple views / view controllers have to display information from the same business logic.

Take for example the an app with a bluetooth connection. There are screens where the user is making the connection 

Like an app with a settings page and a main screen. The settings screen where the user tweaks different settings for an app, and the main screen of the app. With a small application like this, it might make sense for the two viewcontrollers to know about each other and send each other messages. But in a larger app, that sort of design doesn't work as well.

Instead, use a controller created by the AppDelegate. When creating the SettingsScreenViewController, pass in the Controller as an initialization parameter. When creating the MainScreenViewController, pass in the Controller as an initialization parameter. 

When the user interacts with the settings page, the SettingsViewController udpates its views. But also, tells the main app controller about the changes in settings. So when the user taps Back to go back to the main view controller, the MainViewController can update it's views in its own `viewDidAppear` method.

The same can happen in reverse order, too.