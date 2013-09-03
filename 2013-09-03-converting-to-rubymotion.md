At Cyrus, we recently fielded a proposal to convert an existing app on the App Store from Objective-C to RubyMotion. We did quite a bit of work prepping for this proposal. Here are our notes:

Background
What we did
What we recommended


A little background on the proposal. The client already had a pure Objective-C code base. And a live app in the app store. They already had a design firm and a consulting firm that they had a a good working relationship with. They were Rails developers, curious about how RubyMotion could benefit them. If the project were in Ruby, they could understand the code better, and feel confident that they were paying for quality code. 












Because this potential client was a small Rails shop, they only had time to work on their Rails app. All the mobile work would be done mobile-only developers. When hiring for that role, it made sense to them to hire Objective-C programmers because of the perceived larger pool of Objective-C developers to RubyMotion developers. 

Also, they were pushing new features constantly. Stopping development to translate the app to RubyMotion didn't seem like a good use of their time. We would have been happy to implement new features for them. The other firm writing new features while we were translating the app to RubyMotion seemed like a recipe for continual merge conflicts.



Our strategy for the conversion was to vendor the Objective-C project, convert the application delegate to Ruby, and then pull over one controller at a time until we got everything in Ruby that would be likely to change. We planned to leave in Objective-C all the dropped-in third party libraries. 


We started by vendoring the entire Objective-C project. And already hit some bumps in the road. Because the other dev shop used the `.pch` file (pre-compiled headers file) to require fewer `#import` statements in their files, we couldn't simply drop the entire project in a vendor sub-folder in our RubyMotion project. Further, the cocoapods wouldn't load for the vendored project. Because of all these missing dependencies, we had to take a different route. 

Instead, we built the existing project as a static framework for iOS. This meant we didn't have to fiddle with the `.pch` file, muck with pre-compiler directives, or other dependency shenanigans. This gave us a `.a` file that we put in the vendor directory of our RubyMotion project. (Although we found blog posts describing how to do so, we didn't bother making a `.a` file for both the simulator and the iPhone - we were spiking this out to try to get it to work.) Here is the line from our `Rakefile`

    app.vendor_project('vendor/libclientname-static', :static, :products => ['libclientname-static.a'], :headers_dir => "include/libclientname-static", :force_load => false)



After translating the application delegate, vendoring the appropriate included frameworks, and setting up the cocoapods, we had a stable foundation to build on. We were able to reference controllers in the storyboard, and instantiate views from the vendored `.a` file.