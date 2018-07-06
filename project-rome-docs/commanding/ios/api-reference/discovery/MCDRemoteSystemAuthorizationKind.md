---
title: MCDRemoteSystemAuthorizationKind
description: Contains values that describe the potential authorization kinds (user-based authorization scopes) of remote systems.
keywords: microsoft, windows, device relay, how-to iOS, how-to iPhone 
---

# enum `MCDRemoteSystemAuthorizationKind`

```
typedef NS_ENUM(NSInteger, MCDRemoteSystemAuthorizationKind)
```

Contains values that describe the potential authorization kinds (user-based authorization scopes) of remote systems.

|Name | Value | Description |
|:-- |:-- |:-- |
|MCDRemoteSystemAuthorizationKindSameUser | 0 | The system can only discover and connect with devices signed onto by the same user.|
|    MCDRemoteSystemAuthorizationKindAnonymous |1| The system can discover and connect with other users' devices, provided they are in proximity and allow anonymous discovery.|