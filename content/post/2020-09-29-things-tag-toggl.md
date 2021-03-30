---
title: "On productivity"
date: 2020-09-29T20:07:49+10:00
slug: "things-tag-toggl"
description: "My recent thoughts about application-powered productivity."
draft: false
tags: ["Productivity"]
math: false
showToc: true
---

My recent thoughts about application-powered productivity.

<!--mo -->

## Tagging system in Things 3

In my mind, a good tagging system should be a nifty reference when I am **reviewing** my tasks whether it’s in Today tab or I am conducting a weekly thorough review.

The most frequent issue for me is that I am extremely inconsistency in naming tags. Well, the inconsistency is pretty consistent I have to admit. So why not set some restrictions to these tags? Say, I'll just have this number of tags, any attempt to expand this collection will be strictly prohibited.

What I have now is a draft version with only 4 types of tags.

![img](image/things-3-tagging.png)

- **@Prio**. I used to "4-quadrant" everything during my days with Todoist since it’s a good tool with powerful customizable filtering rules. But I guess the quadrants in fact complicate the situations. If something is really urgent, I have to set up a deadline or reminder. If something is not important or urgent, I will review it sooner or later, then I can reevaluate its status. So the only question remains is, what is important? `@Prio` tag is born for handling this.
- **@Status** and 3 sub-tags `Not Started`, `Backlog`, `In Progress`. Some GTD x project management. I need to keep an eye on the progress as well. Could even cut out some tags because I believe the "Someday" in "When" serves basically the same idea as "Not Started". This is just like a potential "Completed" tag versus "Logbook" or "Deleted" tag versus "Trash".
- **@Time** is a collection of nested tags with different time durations. Sometimes I feel it not very practical but have no idea how to improve it.
- **@Media** is just another inbox of different content sources that I can think of. I dump everything here and hope they can make a good course of hodgepodge. They are mainly used as filtering criteria when I want to retrieve one or two out of hundreds of unfinished tasks.

## Toggl categories

### UX issue

It is confusing to add an entry.

First, when I start a timer, it always asks for the description of activity that I like omit since some errands are too miscellaneous to describe.

Second, what does this activity belong to? How to categories my daily life? If I am a first time user of Toggl, I have no clue how to do this. And what is a "project", a "client", a "workspace". These terms used here are too vague and uninstructive at all.

### Solution

The key is to use decisive categories to partition distinctive time uses with as little overlapping as possible.

With a decimal indexing, I can group categories by similar features, and distinguish them by different integer parts as well.

```
- 01 Work (General) 👨‍💻
- 02.1 Intake: Knowledge (General) ⚛️
- 02.2 Intake: Media and Games 🎮
- 02.3 Intake: Reading 🔖
- 03.1 Output: Writing (General) ✏️
- 03.2 Output: Engineering 🛠
- 04.1 Self: Productivity 🚀
- 04.2 Self: Food 🍙
- 04.3 Self: Hygiene 🚿
- 04.4 Self: Workout 💪
- 05 Housework and Cooking 🧹
- 06 Shopping 🥬
- 07 Leisure at Home 💁‍♂️
- 08 Outing and Entertainment 🚗
- 09 Sleeping 💤
```
