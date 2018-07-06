---
title: MCDUserNotificationUserActionStateFilter
description: Contains values that categorize notifications by user action state (for filtered notification retrieval).
keywords: microsoft, windows, device relay, how-to iOS, how-to iPhone 
---

# enum `MCDUserNotificationUserActionStateFilter`

```
typedef NS_ENUM(NSInteger, MCDUserNotificationUserActionStateFilter)
```

Contains values that categorize notifications by user action state (for filtered notification retrieval).

|Name | Value | Description |
|:-- |:-- |:-- |
|MCDUserNotificationUserActionStateFilterNone |0| Include notifications that have not been acted on by the user.|
|    MCDUserNotificationUserActionStateFilterActivated|1| Include notifications that have been activated by the user.|
|    MCDUserNotificationUserActionStateFilterSnoozed|2| Include notifications that have been snoozed by the user.|
|   MCDUserNotificationUserActionStateFilterDismissed|3| Include notifications that have been dismissed by the user.|
|   MCDUserNotificationUserActionStateFilterAny|4| Include notifications regardless of user action state.|