---
title: Structuring iOS Apps
layout: post
published: false
---

I would like to share with you how I like to design my classes for iOS apps. 

1. No Storyboards

I do all my autolayout in code (using (DRAutolayout)[https://github.com/danramteke/DRAutolayout]) because autolayout was really tricky to use in iOS 6's Interface Builder and storyboards. 

Also, code is much easier to search and manipulate.


2. Thinner View Controllers. 

I make my view controllers as the delegate for a screen of content. They handle any animations, updating the views, and any transitions to other screens.

They don't handle logic. 

The view controllers interpret interactions with the user, and then send messages to logic controllers. The logic controllers update the models, perform calculations, trigger services, and, finally, tell the view controllers about any results.

Further, the view controllers are initialized with references to the logic controllers. So on viewWillAppear, they can update their views with the latest state of the world.

Here is some sample code from the AppDelegate:

    self.someService = [[DRSomeSerivce alloc] init];
    self.logicController = [[DRLogicController alloc] initWithSomeService:self.someService];
    self.mainScreenViewController = [[DRMainScreenViewController alloc] initWithController:self.logicController];

    self.navigationController = [[UINavigationController alloc] initWithRootViewController:self.mainScreenViewController]
    self.window.rootViewController = self.navigationController;
    [self.window makeKeyAndVisible];
    return YES;

And when the `MainScreenViewController` navigates to a new screen, it passes it's reference to the `logicController`

    DRSecondScreenViewController* vc = [[DRSecondScreenViewController alloc] initWithLogicController:self.logicController];
    [self.navigationController pushViewController:vc animated:YES];

Now there could be more than one logic controller, of course. Depending on the complexity of the system.