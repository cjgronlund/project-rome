---
title: MCDSyncScope
description: This class represents the user data that is synchronized with the Connected Devices Platform backend when the app uses certain user-specific Connected Devices functionality.
keywords: microsoft, windows, user activities, how-to iOS, how-to iPhone 
---

# class `MCDSyncScope`

```
@interface MCDSyncScope : NSObject
```

This class represents the user data that is synchronized with the Connected Devices Platform backend when the app uses certain user-specific Connected Devices functionality, such as user activities. An instance is retrieved statically from the class that manages such functionality (see **[MCDUserActivityChannel](../useractivities/MCDUserActivityChannel.md)**), and it is used in the construction of **[MCDUserDataFeed](MCDUserDataFeed.md).