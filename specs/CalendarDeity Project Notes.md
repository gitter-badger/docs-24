---
Tags: thoughts
created: 2021-02-18T09:26:41
title: CalendarDeity Spec
---

> This is a thought dump. Don't put too much effort into organizing or editing this info. The goal is to barf ideas out of your brain and then move them to appropriate area: [[CalendarDeity  Project Summary]], [[CalendarDeity Project Propsoal]], or [[CalendarDeity Project Specification]]

## Schedule Event Blocks

- A user's schedule is built every day based on a list of predefined activities. See activity list.
- Each **activity** is essentially a calendar event on the user's mail calendar

 > --> PM/Dev: Just stick with Google APIs for now, but worth exploring other mail providers (especially if a premium version has potential)

- The **Start time** and **End time** are built based on a sequence of events calculated by a `$schedule_starting_time`
- The `$schedule_starting_time` is used to schedule the very first event **Wake Up** activity:
  - By default, this event's starting time is the user's local `$sunrise_time`
  - The user  has the ability to override the wake up activity by

 > --> PM/Dev: There is a working 'Prototype' on [Google Sheets](https://docs.google.com/spreadsheets/d/1lEr_BjRL-OAbjsYw5ORwzz9m2HZPctKO6lUqmTGXgeU/edit?usp=sharing). It's not using any API, but check out the `user_input` tab to understand the very simple override behavior.

## Streams

- Each activity falls into one of three categories that I'm currently calling a **Stream**. They are known as doshas,  or life forces.

- Streams help group activities so that your day is planned based on a ranked sequence (where 1 comes first and 3 is last)

  - Water
    - name: Kapha
    - icon: 💧
    - rank: 1
  - Fire:
    - name: Pita
    - icon: 🔥
    - rank: 2
  - Wind: Vata
    - icon: 🌀
    - rank 3

## Globals

These are some global variables used in reference through the spec. They are listed here to help under stand the process flow of the application.

|path          |via          |key                |value  (e.g.)    |note                                                                                                                                                    |
|--------------|-------------|-------------------|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
|/user/sunrise/|external_api |`$sunrise_time`       |7:10:00 AM |<-- get the user's local sunrise time                                                                                                                   |
|/user/noon/   |external_api |`$solar_noon`         |12:23:00 PM|<-- get the user's local solar noon (meridian)                                                                                                          |
|/user/sunset/ |external_api |`$sunset_time`        |5:36:00 PM |<-- get the user's local sunset time                                                                                                                    |
|/today/       |internal_calc|`$starting_time`     |7:40:00 AM |<-- if user has set start time, use that; otherwise, default to the sun rise time value                                                                 |
|/today/       |internal_calc|`$is_user_started`   |TRUE       |<-- false by default; true when user inputs start time                                                                                                  |
|/today/       |internal_calc|`$total_time_duration`|23:50:00   |<-- Should be near 24 hours; complain and whine if this value is over. Nudge to add more if it's under                                                  |
|/today/       |internal_calc|`$total_events`       |22         |<-- Total events is probably not that important, but be useful to user and we may want to check it doesn't exceed some value... (no more than 48 events)|
|/today/       |internal_calc|`$average_tasks`      |1:05:00    |<-- Some more user feedback                                                                                                                             |

### External API

TBD, lots of time zone options (nice if it could handle fall back to geoIP region for users who don't want to give precise geo-location. Error prone but user's who don't give up location data are probably ok with these errors.

### Internal Calc

There are some internal metrics that would be helpful to display to users. This list is expected to grow as our needs change and there may be room for a dashboard component.

## Today table

The today table lists the user's tentative schedule.

The first activity is always "wake up" and is schedule to be at the user's local time zone's `$starting_time`

- Stream - One of the there Doshas (life forces) [[Ayurvedic practice]]. Possible Ayurveda types are:
  - **Vata** - energy of water and earth (heavy and sluggish). Normally between 6:00am and 10:00am. Good for early morning time-in activities and light meals
  - **Pitta** - energy of fire (vigor and burning). Normally between 10:00 am and 2:00 pm. Propels us to be productive
  - **Kapha**  energy of wind (). Normally between 2:00

- Activity - the name of the activity.
- Start Time - the time the activity should start, calculated from the `$starting_time`
- End Time - the time the activity should end
- Duration - the number of minutes the activity takes to complete
- Matching Event - an event from another calendar that corresponds within that time block. By default, just show the user's local sunrise/sunset. Give options to add other calendars from their mail account.
- Sequence - enumerated collection starting from 0 in arithmetic sequence
Note - a note that can be hinted at a user

### Example Layout

|Stream|Activity                   |Start Time |End Time   |Duration|Matching Event|Sequence|Note|Action|
|------|---------------------------|-----------|-----------|--------|--------------|--------|----|------|
|💧    |Wake up                    |7:10:00 AM |7:40:00 AM |0:30:00 |              |0       |    |prompt|
|💧    |Journal                    |7:40:00 AM |8:10:00 AM |0:30:00 |Sunrise       |1       |    |      |
|💧    |Exercise                   |8:10:00 AM |8:40:00 AM |0:30:00 |              |2       |    |      |
|💧    |Meditate                   |8:40:00 AM |9:00:00 AM |0:20:00 |              |3       |    |      |
|💧    |Eat breakfast              |9:00:00 AM |9:30:00 AM |0:30:00 |              |4       |    |      |
|💧    |Commute or prepare for work|9:30:00 AM |10:00:00 AM|0:30:00 |              |5       |    |      |
|🔥    |Plan your work             |10:00:00 AM|10:30:00 AM|0:30:00 |              |6       |    |prompt|
|🔥    |Focus on important work    |10:30:00 AM|12:30:00 PM|2:00:00 |              |7       |    |      |
|🔥    |Organize your work         |12:30:00 PM|1:00:00 PM |0:30:00 |              |8       |    |      |
|🔥    |Eat Lunch                  |1:00:00 PM |2:00:00 PM |1:00:00 |              |9       |    |      |
|🌀    |Walk or get moving         |2:00:00 PM |2:30:00 PM |0:30:00 |              |10      |    |      |
|🌀    |Communicate important work |2:30:00 PM |4:30:00 PM |2:00:00 |Solar Noon    |11      |    |      |
|🌀    |Finish all work            |4:30:00 PM |5:00:00 PM |0:30:00 |              |12      |    |prompt|
|🌀    |Socialize                  |5:00:00 PM |6:00:00 PM |1:00:00 |              |13      |    |      |
|🌀    |Dinner                     |6:00:00 PM |7:00:00 PM |1:00:00 |              |14      |    |      |
|💧    |Connect                    |7:00:00 PM |8:00:00 PM |1:00:00 |              |15      |    |      |
|💧    |Relax                      |8:00:00 PM |9:00:00 PM |1:00:00 |              |16      |    |      |
|💧    |Reflect                    |9:00:00 PM |10:00:00 PM|1:00:00 |              |17      |    |      |
|💧    |Wind-down                  |10:00:00 PM|10:30:00 PM|0:30:00 |Sunset        |18      |    |prompt|
|💧    |Bedtime                    |10:30:00 PM|11:00:00 PM|0:30:00 |              |19      |    |      |
|🔥    |Deep Sleep                 |11:00:00 PM|3:00:00 AM |4:00:00 |              |20      |    |      |
|🌀    |Light Sleep                |3:00:00 AM |7:00:00 AM |4:00:00 |              |21      |    |      |

## Activity List

Here is a list of preconfigured activities along with their duration. The sequence value is assigned based on a priority (assignable by the user ranked from 1 to 10, where 1 is highest priority).

There is some calculus I don't quite understand how to explain, but the sequence is very simple.

Each activity is assigned a **Stream** and a **Sequence #**. This determines the order each activity is planned throughout the day. Wake-up has is a specialty activity (a1 −
first term) and the only activity where the sequence is set to "0".

Every activity is built incrementally by value of 1  based on this first activity time.

As the stream progresses, the sequence resets to 1 and the app issues a prompt to inquire from the user if they had completed the previous 4 hour block of activities.

- Wake up
  - Sequence: 0
  - Duration: 30 minutes
  - Start-time:
  - Note:
- Journal
- Exercise
- Meditate
- Eat breakfast
- Commute or prepare for work
- Plan your work
- Focus on important work
- Organize your work
- Eat Lunch
- Walk or get moving
- Communicate important work
- Finish all work
- Socialize
- Eat Dinner
- Connect
- Relax
- Reflect
- Wind-down
- Bedtime
- Deep Sleep
- Light Sleep

## Process

On the first sequence ("wake up"), the start time is not set, so I suggest we calculated the first starting time from the user's local sunrise time and offset it by the first event duration -- otherwise, use the previous sequence ending time (as the starting time). Check out user\_input tab for override behavior

every 6 sequences, app prompts if you've done activity (configurable prompt sequence, or turn off entirely).

### Misc

- API endpoints
- /today/ - whole schedule
  - /am/ - just before noon
    - /today/am/
      - /today/am/kapha/
      - /today/am/pitta/
      - /today/am/vata/
  - /pm/ - everything else afternoon
    - /today/pm/kapha/
    - /today/am/pitta/
    - /today/am/vata/
-/tasks/
  - /suggestion/
  -/meditating/
 -/custom/
  "i have a lot of shit to do and don't know when to do them"

tasks  {
 priority: 3, 2, 1
 bucket: fire, water, wind
 duration: min, max (4)
}

bucket {
 name: fire | wind | water
 meridiem: am | pm
}
