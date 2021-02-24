---
Tags: product, design, plans
created: 2021-02-23T14:17:39
Audience: Engineering and Development
Description : Development plans, technical details, resources, etc
title: CalendarDeity Project Specification
---

> **How** the project is implemented (technical details and resources)

## Open API Spec

For database schemes and routes, request body objects, error handling

- Check the [spec.yaml](spec.yaml) [#to-do]

## Components

## Scheduler

The main component is the scheduler - this allows the user to set their preferred sequence of time blocks.

The time block itself is just a calender event (i.e., on google calender)

Each Time block has a user-definable name and is assigned to a user-defined group of "Streams"

> PM/DEV --> I'm not postivie how this should work yet, there's some arguements that coudl be made that some time-blocks don't belong in some streams -- i will work out that logic in the [[CalendarDeity Project Summary]]

The first stream starts at the same time as the scheduled starting time.

- The starting team default to the user's sunrise time, but can be manually edited by the user to a specific time

Each block has an status `$is_scheduled` which enables or disables the time block from appearing on the user's calendar.

The block can also be configured to set a reminder (which should fall back to native calendar reminder)

> PM/DEV --> Here is a rough mock up i did on wdyt.pro. But I know there's some other mockup tool you're using - let me know what you prefer
>
> - <https://wdyt.pro/share/U2FsdGVkX18Gyh0kmCjJF%2Btfid2s46pK9yjbnRrSoY2hX6oWMOV19K%2B6GUt4EgwS>

![[specs/exhibits/rough_scheduler.png]]

## Calendar Integration

Each window of time is a block. Within the block, users can schedule new events, like meetings, or can create tasks that fit within that block.

![[specs/exhibits/gmail_user.png]]

<small>Example of what a user sees on their calendar</small>

## Technical Frameworks

- [#to-do]
