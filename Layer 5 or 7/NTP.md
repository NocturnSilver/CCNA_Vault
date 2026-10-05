#layer5

## Context
- all devices have an internal clock

## Troubleshooting
- the hardware calendar is the default-time source, however, the hardware clock and the software clock are separate and can be configured separately.
- It is recommended to sync the clock and the calendar.
## What is NTP

## Why do we use NTP?
- accurate time on a device gives accurate logs for troubleshooting

## Commands

### Clock Config Commands
| Number | Reason                                       | Command                                                            |
| ------ | -------------------------------------------- | ------------------------------------------------------------------ |
| 1      | Manually set the time one the device         | R# clock set \[hh:mm:ss] <1-31> \<Jan-Dec> \<YYYY>                 |
| 2      | Manually set the hardware clock              | # calendar set \[hh:mm:ss] <1-31> \<Jan-Dec> \<YYYY>               |
| 3      | Synchronise the calendar to the clock time   | # clock update-calendar                                            |
| 4      | Synchronise the clock to the calendar's time | # clock read-calendar                                              |
| 5      | Configure the timezone                       | # clock timezone \[time zone] <br><-23 - 23 hours offset from UTC> |


### Show commands

| Number | Reason                              | Command             |
| ------ | ----------------------------------- | ------------------- |
| 1      | shows the time, timezone, day, date | # show clock        |
| 2      | shows more including time source    | # show clock detail |
