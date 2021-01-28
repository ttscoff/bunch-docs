---
layout: default
title: Scheduling Bunches
parent: Bunch Files
---
# Scheduling Bunches

You can use Bunch like an alarm clock or timer by making use of some [frontmatter keys](/bunch/docs/bunch-files/frontmatter) in your Bunch files.

| `open at:`    | Set a time to open this bunch daily    |
| `close at:`   | Set a time to close this bunch daily   |
| `open on:`    | Set a weekday and time to open weekly  |
| `close on:`   | Set a weekday and time to close weekly |
| `open every:` | Repeat open at intervals               |

Bunch will always read in these keys and set the alarms and timers when it launches, so it doesn't matter if you quit the app in between scheduled launches.

## Open at intervals

The `open every` key runs the Bunch at timed intervals. The value should be shorthand for hours and minutes to create an interval: `1h30m` would run it every hour and a half. You can also just use `1h` or `30m`. 

You can also use "d" to specify days. If you want to launch every other day, use "2d". This, however, does not allow you to specify a time. So, for most intents and purposes you'll want to use `open at`.

It will even let you do seconds (`s`), if you needed to.

```
---
open every: 1h30m
---
```

This can be useful for always-open Bunches, though it can be disruptive if launching apps and opening files takes window focus from what you're working on. This is most useful for small Bunches that use things like [Spotlight searches](/bunch/docs/bunch-files/spotlight-searches) to open files, allowing them to be continually updated.

## Daily Schedules

The "at" commands trigger daily.

### Opening Daily

The `open at` key creates a daily "alarm" that will go off at the same time every day, as long as Bunch is running.

```
---
open at: 5pm
---
```

Time can be specified with a meridian (am/pm) or as 24 hour time. Whatever time it initially goes off, it will then start repeating at 24 hour intervals. If Bunch isn't running when the time comes, it will not launch automatically again until the next day.

### Closing Daily

You can also use `close at` to close a Bunch at a set time each day. `open at` and `close at` can be used simultaneously.

## Weekly Schedules

The "on" commands trigger weekly.

### Opening Weekly

You can specify a day of the week and a time to create weekly Bunches, great for end-of-the week reviews, or celebrating the weekend by shutting down Slack.

```
---
open on: friday 5pm
---
!Slack
```

### Closing Weekly

You can also use `close on` to close a Bunch weekly at a set day and time. `open on` and `close on` can be used simultaneously.

## Natural Language Dates

All of the scheduling keys (other than `open every`) allow natural language dates and times. You can just write `1pm` or `tue noon` and it should figure out what you're trying to do. You can view the Console to see the confirmation that scheduling is happening. I don't currently offer a front-end way to see what all is scheduled, but I'd like to eventually.

## Cancelling an Automatic Launch or Close

{% img mx-auto /bunch/images/bunchcancel.gif 806 454 %}

When launching and closing Bunches on a schedule, Bunch will attempt to show a notification 15 seconds before the action happens. Clicking the notification will cause it to the action to happen immediately, and there's a cancel button to skip that scheduled launch until the interval comes around again. This notification system is only tested on Big Sur. I highly recommend setting Bunch's notification style to "Alert" in System Preferences, they work much better for what Bunch uses them for.