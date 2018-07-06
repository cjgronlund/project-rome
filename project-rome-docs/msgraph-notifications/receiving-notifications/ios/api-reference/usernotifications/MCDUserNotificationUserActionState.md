---
title: MCDUserNotificationUserActionState
description: Contains values describing the action a user has taken on a notification.
keywords: microsoft, windows, device relay, how-to iOS, how-to iPhone 
---

# enum `MCDUserNotificationUserActionState`

```
typedef NS_ENUM(NSInteger, MCDUserNotificationUserActionState)

```

Contains values describing the action a user has taken on a notification.

|Name | Value | Description |
|:-- |:-- |:-- |
|MCDUserNotificationUserActionStateNone |0| The user hasn't taken any action.|
|   MCDUserNotificationUserActionStateActivated|1|The user has activated the notification.|
|    MCDUserNotificationUserActionStateSnoozed|2| The user has snoozed the notification.|
|   MCDUserNotificationUserActionStateDismissed|3| The user has dismissed the notification.|