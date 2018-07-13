## Register app with the cloud directory

App registration is done through the **MCDRemoteSystemApplicationRegistration** class. The following code registers the app; you can put it directly below the platform initialization code. 

```ObjectiveC
- (void)initializePlatform
{
    // ...

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