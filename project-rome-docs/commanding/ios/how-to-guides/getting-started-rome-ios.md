---
title: Getting started with Connected Devices (iOS)
description:  Remotely launch a Universal Windows Platform (UWP) app or Windows desktop app from an iOS device with Project Rome.
keywords: microsoft, windows, project rome, iOS, iOS api reference, iPhone 
---

# Getting started with Connected Devices (iOS)
This guide shows you how to set up the Connected Devices Platform on an iOS app and use it to discover and connect to remote devices and apps. Refer to the [iOS sample app](https://github.com/Microsoft/project-rome/tree/master/iOS/sample) for a working example of all iOS scenarios.


## Preliminary setup for Connected Devices functionality

Before implementing remote connectivity, there are a few steps you'll need to take to give your iOS app the capability to connect to remote Windows devices.

### Sign-in

Microsoft Account (MSA) authentication is required for all features of the SDK, other than the Nearby Sharing APIs. 

If you do not already have an MSA, Register for one on [account.microsoft.com](https://account.microsoft.com/account).

The Project Rome SDK requires an OAuth 2.0 access token to register with the platform. An OAuth 2.0 access token can be obtained using the developer's preferred method. In an attempt to help developers onboard with the platform more easily, we have provided an authentication provider as part of the  
iOS and iOS samples. The [sample authentication provider](https://github.com/Microsoft/project-rome/tree/master/iOS/samples/account-provider-sample) can be used to obtain the OAuth 2.0 access token and refresh token for your app.

Next, you must register your app with Microsoft by following the cross platform instructions on the [Application Registration Portal](https://apps.dev.microsoft.com/) (if you do not have a Microsoft developer account created, you must do this first). You should receive a client ID string for your app - save this for later. This will allow your app to access Microsoft's Connected Devices Platform resources. 

The simplest way to add the Connected Devices platform to your iOS app is by using the [CocoaPods](https://cocoapods.org/) dependency manager. Go to your iOS project's *Podfile* and insert the following entry:

```ObjectiveC
platform :ios, "10.0"
workspace 'iOSSample'

target 'iOSSample' do
  # Uncomment the next line if you're using Swift or would like to use dynamic frameworks
  # use_frameworks!

	pod 'ProjectRomeSdk'

  # Pods for iOSSample
```

> **Note:** In order to consume CocoaPod, you must use the _.xcworkspace_ file in your project.

## Initialize the Connected Devices platform

Before any Connected Devices features can be used, the platform must be initialized. 

The **MCDPlatform**'s `platformWithAccountProvider:` method takes two parameters: a **MCDUserAccountProvider** and a **MCDNotificationProvider**. The **MCDNotificationProvider** parameter is only needed for remote app hosting and User Activities, which are not covered in this guide. It can be left `null` for the scenarios here.


The **MCDUserAccountProvider** is needed to deliver an ID for the current user to the Connected Devices Platform. It will be called the first time the app is run and upon the expiration of a platform-managed refresh token. This is where the class(es) in the [sample authentication provider](https://github.com/Microsoft/project-rome/tree/master/iOS/samples/account-provider-sample) can be used. It provides classes that implement the **MCDUserAccountProvider** interface and facilitate the account-fetching process.

The final step of initialization involves registering the current application with the Connected Devices Platform cloud directory through the use of the **MCDRemoteSystemApplicationRegistration** class.

The following code from the sample app shows the initialization of the platform.

```ObjectiveC
- (void)initializePlatform
{
    // Only register for APNs if this app is enabled for push notifications
    NotificationProvider* notificationProvider;
    if ([[UIApplication sharedApplication] isRegisteredForRemoteNotifications])
    {
        notificationProvider = [AppDataSource sharedInstance].notificationProvider;
    }
    else
    {
        NSLog(@"Initializing platform without a notification provider!");
        notificationProvider = nil;
    }

    // Initialize platform
    [AppDataSource sharedInstance].platform = [MCDPlatform platformWithAccountProvider:[AppDataSource sharedInstance].accountProvider notificationProvider:notificationProvider];

    // App is registered asynchronously.
    MCDHostingRemoteSystemApplicationRegistrationBuilder* builder = [MCDHostingRemoteSystemApplicationRegistrationBuilder new];
    [builder setLaunchUriProvider:[[LaunchUriProvider alloc] initWithDelegate:[AppDataSource sharedInstance].inboundRequestLogger]];
    [builder addAppServiceProvider:[[AppServiceProvider alloc] initWithDelegate:[AppDataSource sharedInstance].inboundRequestLogger]];
    [builder addAttribute:@"ExampleAttribute" forName:@"ExampleName"];
    MCDRemoteSystemApplicationRegistration* registration = [builder buildRegistration];
    
    [registration addCloudRegistrationStatusChangedListener:^(MCDUserAccount * _Nonnull account, MCDCloudRegistrationStatus status) {
        NSLog(@"Cloud Registration Status Changed listener");
        switch (status) {
            case MCDCloudRegistrationStatusFailed:
                NSLog(@"Cloud registration completed with status Failed");
                break;
            case MCDCloudRegistrationStatusInProgress:
                NSLog(@"Cloud registration in progress");
                break;
            case MCDCloudRegistrationStatusNotStarted:
                NSLog(@"Cloud registration not started");
                break;
            case MCDCloudRegistrationStatusSucceeded:
                dispatch_async(dispatch_get_main_queue(), ^{
                    // The app has been registered.  It is safe to enable button.
                    [self.deviceRelayButton setEnabled:YES];
                    [self.activityFeedButton setEnabled:YES];
                });
                break;
        }
    }];
    
    [registration start];
}
```

You should shut down the platform when your app exits the foreground by calling the `shutdownAsync:` method.

> Important: As this is a preview release, there are some known bugs in the Project Rome platform. Currently, shutting down the Connected Devices Platform will cause the app to crash. This is trivial if it is only done when the app is exiting, but we are working on a timely fix. 

At this point, your next actions will depend on which scenario(s) you intend to implement. For Device Relay scenarios, such as launching an app on a remote device or communicating with an app service on a remote device, see the [Command remote devices and apps](command-remote-devices-and-apps-iOS.md) guide (this also covers nearby sharing with other devices). For User Activities scenarios, such as creating, publishing, and reading Windows user activities, see the [Publish and read user activities](user-activities-iOS.md) guide. To make your app a host (able to be remotely launched or deliver app service resources to a remote device), see the [Host cross-device experiences](hosting-iOS.md) guide. 
