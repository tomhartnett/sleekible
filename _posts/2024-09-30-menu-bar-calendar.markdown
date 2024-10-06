---
layout: post
title: "Menu bar calendar app"
date: 2024-09-30 00:00:00 -0500
categories:
---

I like having a quick way to view a calendar—not my scheduled appointments and meetings, but a simple view of the current month and its dates. This seems like the perfect opportunity for a menu bar app on macOS, where I can click to drop down a window showing the calendar. Unfortunately, macOS doesn’t include this feature out of the box. While there are many such menu bar apps available in the Mac App Store (and I used one for a while), I decided to build my own for fun.

I called it simply "MenuBarCalendar" and built it with SwiftUI using the `MenuBarExtra` scene that was new with macOS 13. The repo is public and available [on GitHub](https://github.com/tomhartnett/MenuBarCalendar). The app displays a simple calendar and a couple menu items. You can use the arrows at the top navigate to past and future months.

![Screenshot of the MenuBarCalendar app](/assets/menubarcal-screenshot.png)

The basic code to show a menu bar item is pretty simple. You use the `MenuBarExtra` scene. In it,  you add items to show in the drop down menu. Here I am adding three items: `MonthCalendarView`, `TodayView`, and `QuitView`. (Each item is separated by a `Divider`).

```
    var body: some Scene {
        MenuBarExtra(isInserted: .constant(!isPreview), content: {
            VStack(alignment: .leading, spacing: 8) {

                // Shows the month view
                MonthCalendarView()
                    .environmentObject(context)
                    .environmentObject(monthViewModel)

                Divider()

                // Menu item showing the current date
                TodayView()
                    .environmentObject(context)

                Divider()

                // Menu item for quitting the app
                QuitView()
            }
            .padding(.all, 8)
            .onAppear { ... }

        }, label: {
            Image(systemName: "calendar.badge.clock")
        })
        .menuBarExtraStyle(.window)
    }
```

One challenge I faced is that in order to show a complex view (something other than a simple text menu item), you have to apply the `.menuBarExtraStyle(.window)` modifier (last line in the above code). The default style is `.menu`, but I needed `.window` in order to show the month view. But with `.window`, you don't get the built-in highlighting of menu items. Besides the view of the current month (which requires the window style), I wanted a menu item showing the current date (for navigating back to it), and an option to quit the app. I wanted these two items to highlight when you mouse over them. You get this highlighting behavior for free with the `.menu` style, but not with `.window`. To simulate it, I used the `.onHover` view modifier to detect when the mouse pointer moved over the menu item. I tracked this with a `@State` variable and set the `foregroundStyle` and `background` of the item based on that state. The code is below:

```
@State private var isMouseOver: Bool = false

var body: some View {
    HStack {
        Text("Quit")
            .foregroundStyle(isMouseOver ? Color.white : Color.primary)
            .padding(.leading, 8)

        Spacer()
    }
    .frame(maxWidth: .infinity, minHeight: 22)
    .background(isMouseOver ? Color.accentColor.opacity(0.75) : Color.clear)
    .cornerRadius(4)
    .onHover { isOver in
        isMouseOver = isOver
    }
    .onTapGesture {
        NSApplication.shared.terminate(nil)
    }
}
```

The result works fairly well:

![Demo video showing the menu item highlighting](/assets/menubarcal-demo.gif)

