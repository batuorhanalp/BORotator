Since Apple has announced its 4th generation of Apple TV, we can develop applications for the tvOS. Of course we cannot expect tvOS SDK to be as capable as iOS one. Still the SDK needs some improvements one of them being enabling access to the rotation data from the Siri Remote.

As Apple document said we can get swipe, tap gestures; even low level gestures like “press began”, “press ended”…

Since Siri Remote has gyroscope we can also get movement data of remote, except rotation angle! On Apple’s GCMotion document for tvOS, we still can’t get rotation angle of remote.

Just call Remote shared instace from AppDelegate.
```
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool 
{ 
    NSNotificationCenter.defaultCenter().addObserver(self, selector: #selector(controllerDidConnect), name: GCControllerDidConnectNotification, object: nil) 
    return true 
}
func controllerDidConnect(notification: NSNotification) { 
    let gameController = notification.object as? GCController 
    if gameController != nil 
    { 
        Remote.sharedInstance.controller = gameController 
    } 
}
```


In your view controller track your remote motion with valueChangedHandler.
```
if let controller = Remote.sharedInstance.controller { 
    if let motion = controller.motion { 
        motion.valueChangedHandler = { (motion) -> Void in 
            let angle = controller.rotation(motion)
        } 
    } 
}
```
