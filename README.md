[MeowlWatch] Schedules
======================

This repo contains the files that [MeowlWatch] uses to display dining schedules.

See [CONTRIBUTING.md] to contribute.

- [CafeSchedules.plist]
- [DiningHallSchedules.plist]
- [NorrisSchedules.plst]

[MeowlWatch]: https://itunes.apple.com/us/app/meowlwatch-for-northwestern-university-dining/id1219875692?mt=8
[CONTRIBUTING.md]: CONTRIBUTING.md
[CafeSchedules.plist]: CafeSchedules.plist
[DiningHallSchedules.plist]: DiningHallSchedules.plist
[NorrisSchedules.plst]: NorrisSchedules.plist

Interpreting Schedule Files
---------------------------

Schedule files are represented in roughly the following way:

```xml
<dict>
    <key>NAME OF DINING LOCATION</key>
    <array>
        <dict>
            <key>StartingDayOfWeek</key>
            <string>THE STARTING DAY OF WEEK</string>
            <key>EndingDayOfWeek</key>
            <string>THE ENDING DAY OF WEEK</string>
            <key>Schedule</key>
            <dict>
                <key>TIME</key>
                <string>STATE</string>
                <!-- ... -->
            </dict>
        </dict>
        <!-- ... -->
    </array>
</dict>
```

Because weeks are cyclical, we represent the schedule for a dining location with a list of `StartingDayOfWeek`s and `EndingDayOfWeek`s, and relevant times of day within those days.

Because days are also cyclical, we represent times with 24-hour numeral representations.

- NAME OF DINING LOCATION: The name of a dining hall or café, like `Allison`.
- `StartingDayOfWeek` and `EndingDayOfWeek`: Days of the week like `Monday` or `Tuesday`.
- TIME: a 24-hour representation of a time of day, like `0800` for 8:00 AM or `2345` for 11:45 PM.
- STATE: `Open` or `Closed`, or `Dinner` or something for a dining hall.

In accordance with [Apple's documentation](https://developer.apple.com/documentation/foundation/nsdatecomponents/1410442-weekday), `Sunday` is the first day of the week, and `Saturday` is the last.

The TIME is when the dining location changes from that state, and that state remains until the next TIME. We end every day with `2400`, with whatever state it is in.

Example
-------

Subway is open 11:00 AM to 9:00 PM every day of the week.

```xml
<key>Subway</key>
<array>
    <dict>
        <key>StartingDayOfWeek</key>
        <string>Sunday</string>
        <key>EndingDayOfWeek</key>
        <string>Saturday</string>
        <key>Schedule</key>
        <dict>
            <!-- Closed from 00:00 to 11:00 -->
            <key>1100</key>
            <string>Closed</string>

            <!-- Open from 11:00 to 21:00 -->
            <key>2100</key>
            <string>Open</string>

            <!-- Closed until end-of-day -->
            <key>2400</key>
            <string>Closed</string>
        </dict>
    </dict>
</array>
```

Frontera is open from 11:00 AM to 3:00 PM, but only on Mondays to Fridays.

Although we usually have to start the week on `Sunday`, I have made an exception to my code to handle this.

```xml
<key>Frontera Fresco</key>
<array>
    <dict>
        <key>StartingDayOfWeek</key>
        <string>Monday</string>
        <key>EndingDayOfWeek</key>
        <string>Friday</string>
        <key>Schedule</key>
        <dict>
            <key>1100</key>
            <string>Closed</string>
            <key>1500</key>
            <string>Open</string>
            <key>2400</key>
            <string>Closed</string>
        </dict>
    </dict>
    <dict>
        <key>StartingDayOfWeek</key>
        <string>Saturday</string>
        <key>EndingDayOfWeek</key>
        <string>Sunday</string>
        <key>Schedule</key>
        <dict>
            <!-- Closed 00:00 to end-of-day -->
            <key>2400</key>
            <string>Closed</string>
        </dict>
    </dict>
</array>
```
