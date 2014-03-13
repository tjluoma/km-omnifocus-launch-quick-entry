On [Mac Power Users 181](http://www.macpowerusers.com/2014/03/09/mac-power-users-181-automation-workflows-with-tj-luoma/), I mentioned that I have a keyboard shortcut for OmniFocus’ “Quick Entry” which works *even if OmniFocus isn’t running*. [Tom Siko](https://twitter.com/tcsiko/status/443803903818072064) asked if I’d be willing to share it, so here it is.

**Even if you don’t use OmniFocus,** this idea can be re-used for *any application that you run via keyboard shortcut*, such as [Skitch](http://evernote.com/skitch/) (or, my preference, [Skitch 1](http://evernote.com/download/get.php?file=SkitchMac_v1)).

### First, here’s the general idea…

The idea behind this is fairly simple: 

Imagine you have some keyboard shortcut in your head, such as <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>5</kbd> for Skitch, or <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>O</kbd> for “Quick Add” to OmniFocus. Over time, you learn that whenever you want to do *{thing}* you press *{this keyboard shortcut}*. But there’s one problem: those keyboard shortcuts only work if the app is running. So what do you do? The usual solution is to have those apps (or some kind of “helper app”) launch at login. But why have something launch before you need to use it? Especially with an SSD, launching an app only when it’s needed will only take a second or two.  Likewise, why _keep_ an app running once you are done using it?

Rather than have a bunch of these apps launch at login, I created several Keyboard Maestro macros to watch for the keyboard shortcuts, and then check to see if the related app is running. If it is, then Keyboard Maestro does nothing. If it isn’t, then Keyboard Maestro will launch the app _and_ trigger the action that was associated with the keyboard shortcut.

The Keyboard Maestro “workflow” for this is very straightforward:

When a certain keyboard shortcut (or “hot key” in Keyboard Maestro terminology) is pressed, have Keyboard Maestro run an “If Then Else” test:

***If*** the desired app is *not* running, ***then:*** 

*	Launch it
*	Pause until it is running
* 	Activate _YourApp_
* 	Trigger the desired action

***else*** (that is, if _YourApp_ is running):

* 	{This section intentionally left blank}
 
The “Else” section is left empty, because if the app _is_ running, we don’t want Keyboard Maestro to do anything when the keyboard shortcut is pressed, because _YourApp_ will.

### “OK, I get the idea, show me how you do it…”

Some of you may already understand how to do this, but some of you may want to see how it actually looks in Keyboard Maestro. So here is a screenshot of my Keyboard Maestro macro for Skitch:

<img alt="Keyboard Maestro macro for Skitch" data-caption="" data-credit="AOL" data-dam-provider="aol" data-local-id="local-1" data-media-id="6590427" data-original-url="http://o.aolcdn.com/hss/storage/adam/4a0c66f1f9c8ee1af5810ee71708fb41/km-skitch-tuaw.jpg" data-title="" src="http://o.aolcdn.com/hss/storage/adam/4a0c66f1f9c8ee1af5810ee71708fb41/km-skitch-tuaw.jpg" />

*Note: [a larger version of this image](https://github.com/tjluoma/km-omnifocus-launch-quick-entry/blob/master/img/km-skitch.png) is available on Github.*

Each one of those “blocks” in Keyboard Maestro is just something that I have clicked and dragged from Keyboard Maestro’s list of available actions, and selected from the various drop-down menus. Building something like this is really not much more difficult than writing a Mail.app rule.

With any multiple-step macro like this, the key to making it work is to make sure that “Step 2” doesn’t try to run before “Step 1” has a chance to finish, and so on. Keyboard Maestro lets you choose between either waiting for a certain number of seconds, or waiting until certain conditions are met. For example, if I could tell Keyboard Maestro to launch (or “activate”) Skitch, and then tell it to pause for 5 seconds before going on to the next step. I tend to prefer using _conditions_ because they are a little less error-prone. If your computer is doing something else which makes it a little slower, it may take longer than 5 seconds, or it may take fewer than 5 seconds, especially if your computer is idle and has an SSD.

So in the macro I told Keyboard Maestro to launch Skitch and then wait until there is a menu item called “Crosshair Snapshot” (which is what Skitch calls the command I associate with <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>5</kbd>). When Keyboard Maestro sees that menu item available, I know that Skitch is now ready, so I tell Skitch to select the menu item “Crosshair Snapshot” from the “Capture” menu in Skitch. (Note: I didn't have to fill in the menu title or menu item names either. Keyboard Maestro did that when I clicked on the "Menu” button near the right. It shows me all currently available apps and all of their menus, so I just have to select the right one, and it fills in the exact details for me.)

At the bottom of that macro window you see that the “otherwise execute the allowing actions” section is blank (“No Action”). This is the “Else” part of the “If Then Else” which started with “If Skitch is not running” so logically this section will only match if Skitch _is_ running, in which case Skitch will see that I have pressed <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>5</kbd> and respond accordingly.

After I have used Skitch I can either leave it running if I think I might use it again, or I can just quit it, knowing that Keyboard Maestro will launch it again later if necessary.

***Note!*** Here’s one important potential “gotcha” for this: Skitch has an option in its preferences which will allow it to run _only_ in the menu bar and _not_ in the Dock. If you choose _not_ to have it run in the dock, it will _not_ have menu items that Keyboard Maestro can access! What do you do in that scenario?  Just change Keyboard Maestro’s _conditional_ so that (instead of pausing until it finds a menu item) it will “Pause until Skitch is running” and then tell Keyboard Maestro to simulate the keyboard shortcut <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>5</kbd>! Did I just blow your mind a little?  You press <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>5</kbd> and then having Keyboard Maestro press <kbd>⌘</kbd>+<kbd>⇧</kbd>+<kbd>5</kbd> again. It’s like having a sandwich that can make you another sandwich.

### OmniFocus ###

My keyboard shortcut for OmniFocus Quick Entry is <kbd>⌃</kbd> + <kbd>⌥</kbd>  + <kbd>⌘</kbd> + <kbd>O</kbd>, which might sound crazy, but as I explained on MPU, I have set my ***right*** <kbd>⌥</kbd>  key and <kbd>Caps Lock</kbd> to equal <kbd>⌃</kbd> + <kbd>⌥</kbd>  + <kbd>⌘</kbd>, so when I want to add something to OmniFocus, I press <kbd>Caps Lock</kbd> + <kbd>O</kbd> or <kbd>Option_R</kbd> + <kbd>O</kbd>.

The key is to make sure that whatever keyboard shortcut you have in OmniFocus’ preferences here:

<img alt="OmniFocus Quick Entry Preferences" data-caption="" data-credit="AOL" data-dam-provider="aol" data-local-id="local-3" data-media-id="6590467" data-original-url="http://o.aolcdn.com/hss/storage/adam/ec0ec1ce84dff4499eb0298aec71942c/OmniFocus-Quick-Entry-Shortcut-tuaw.jpg" data-title="" src="http://o.aolcdn.com/hss/storage/adam/ec0ec1ce84dff4499eb0298aec71942c/OmniFocus-Quick-Entry-Shortcut-tuaw.jpg" />

matches whatever you use in the associated Keyboard Maestro macro under “This hot key”:

<img alt="Keyboard Maestro macro for OmniFocus" data-caption="" data-credit="AOL" data-dam-provider="aol" data-local-id="local-4" data-media-id="6590470" data-original-url="http://o.aolcdn.com/hss/storage/adam/6a0d04c095b91a28e88b9cad12d4e842/km-omnifocus-tuaw.jpg" data-title="" src="http://o.aolcdn.com/hss/storage/adam/6a0d04c095b91a28e88b9cad12d4e842/km-omnifocus-tuaw.jpg" />

*As before, [a larger version of this image is on Github](https://github.com/tjluoma/km-omnifocus-launch-quick-entry/blob/master/img/km-omnifocus.png).*

Same idea as before: if OmniFocus is _not_ running, activate it, pause until it is running and a “Show Quick Entry” menu item exists, and then select the menu item.

If OmniFocus _is_ running, Keyboard Maestro does nothing, and the Quick Entry window will simply appear as usual.

These are only two examples, but the same idea applies to *any* application that you invoke via a keyboard shortcut. It is a handy way to avoid having a lot of apps launch at login, and allows you to only have them running when you need them.

### Download and Install ###

If you would like to use my Keyboard Maestro macros as shown above, [download the zip file from Github](https://github.com/tjluoma/km-omnifocus-launch-quick-entry/archive/master.zip), unzip it, and inside you will find both .kmmacros files. Double-click on them to import into Keyboard Maestro, and you’re ready to go!

Any updates to these macros will be posted [on the Github page](https://github.com/tjluoma/km-omnifocus-launch-quick-entry), and if you have any problems getting these to work, you can [send them to me there](https://github.com/tjluoma/km-omnifocus-launch-quick-entry/issues) or [ask me on Twitter](http://twitter.com/tjluoma). If you haven’t already listened to [Mac Power Users 181](http://www.macpowerusers.com/2014/03/09/mac-power-users-181-automation-workflows-with-tj-luoma/), I share a whole host of other nerdy automation tips there too.