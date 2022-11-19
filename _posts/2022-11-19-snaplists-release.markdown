---
layout: post
title: "New Sorting Options and Settings Sync"
date: 2022-11-19 00:00:00 -0600
categories:
---

A user of the Snaplists app provided feedback recently that she would like to be able to control the sort order of the lists. Since 1.0, the lists have been sorted by last modifed descending, so any time a list is edited, it becomes the top-most list. I thought it was a good idea, so I added some new sort options to the app.

In version 2022.11, the user can now choose to sort the lists by last modified, name, or manual. The user can also drag and re-order the lists as desired (this is the "manual" sort order). 

![screenshot of sorting options](/assets/sort-options-and-edit-mode.png)

I wanted the new sort preference to sync automatically to the watchOS app (as well as any other devices), so the user sees their lists in the expected order there as well. But how to sync the setting to the watch? It would have been super simple if I could have used `NSUbiquitousKeyValueStore` but unfortunately, that is not available on watchOS. _Well actually, it is available on watchOS 9, but I'm still supporting watchOS 8._

I decided to add another entity to the Core Data model to sync the sort preference across devices. I wanted to add something that might be useful in the future for other settings that might need to be synced, so I named the new entity `AppSetting`. It has an `Integer 64` property for storing the setting value.

![screenshot of app setting entity](/assets/appsetting-entity.png)

In my Swift code, I have an enum backed by `Int64` and I'm storing the `rawValue` of the enum in the AppSetting entity when I save it to Core Data.

```
public enum SMPListsSortType: Int64 {
    case lastModifiedDescending = 0
    case nameAscending = 1
    case manual = 2
}

extension SMPListsSortType {
    static var title: String {
        "List Sort Type"
    }

    static var id: UUID {
        UUID(uuidString: "00000000-0000-0000-0000-000000000003")!
    }
}
```

There's two methods for reading and saving the setting to the database. These methods deal with the AppSetting entity internally and just expose the enum to the calling code. So as far as the calling code knows, it's just reading/saving the sort preference of type `SMPListsSortType` while it is an AppSetting entity being saved to Core Data.

```
public func getListsSortType() -> SMPListsSortType {
    if let entity = getAppSettingEntity(with: SMPListsSortType.id),
        let sortType = SMPListsSortType(rawValue: entity.intValue) {
        return sortType
    }

    return SMPListsSortType.lastModifiedDescending
}

public func updateListsSortType(_ sortType: SMPListsSortType) {
    let entity: AppSettingEntity
    if let existingEntity = getAppSettingEntity(with: SMPListsSortType.id) {
        entity = existingEntity
    } else {
        entity = AppSettingEntity(context: context)
    }

    entity.intValue = sortType.rawValue
    entity.identifier = SMPListsSortType.id
    entity.title = SMPListsSortType.title

    saveChanges()
}
```

In the above code, `SMPListsSortType.id` is a static property that returns a hard-coded UUID, so there should only ever be one of these settings saved to the database. And in the `getListsSortType` method, if an entity is not found (because the user has never changed the sort order), then it defaults to sorting by last modified.

Also in this release, I added an option to toggle the automatic sorting of items on a list. Previously, as items were checked, they moved towards the bottom of the list automatically. Now the user can choose to toggle that behavior on/off.

![screenshot of items sorting preference](/assets/automatically-sort-items.png)

And finally, I added a [build configuration (.xcconfig)](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=_8) file to the Xcode project. I am using this to set the `MARKETING_VERSION` setting, so now I only need to set that in one place. Previously, I had to update the version on the iOS app, watchOS app, a framework, the widget extension, and an intents extension! That was five places I had to remember to bump the version when preparing a release. Now, I can just set it in the .xcconfig file 🙂

Version 2022.11 of Snaplists, with new list sorting options, is now available in the App Store.

[![Download Link](/assets/Download_on_the_App_Store_Badge_US-UK_RGB_blk_092917.svg)](https://apps.apple.com/mk/app/snaplists-simple-lists-app/id1527429580)