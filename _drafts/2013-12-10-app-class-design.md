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

When I have a globally used service, I do the same thing. Like a class that managers a bluetooth connection, or a location manager. I instantiate in the AppDelegate the manager class and pass it into view controllers as I create them. That way, all concerned view controllers know about the service. And so does the AppDelegate. So when the AppDelegate gets a message from the operating system, the AppDelegate can easily tell the service.

Maybe some code?

    self.controller = [[DRAppController alloc] init];
    self.presetProvider = [[DRPresetProvider alloc] init];
    self.selectionController = [[DRSelectionViewController alloc] initWithController:self.controller andProvider:self.presetProvider];

    self.navigationController = [[UINavigationController alloc] initWithRootViewController:self.selectionController]
    self.window.rootViewController = self.navigationController;
    [self.window makeKeyAndVisible];


The AppController and the PresetProvider are my model controllers. And you could say the represent the backend of my app. The navigation