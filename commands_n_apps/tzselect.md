# `tzselect` - select a timezone

> [!quote] &nbsp;Excerpt from man page:
> 
> ```
> DESCRIPTION
> The tzselect program asks the user for information about the current location, and outputs the resulting timezone to standard output. The output is suitable as a value for the TZ environment variable.
> ```

The `tzselect` tool asks a few questions and at the end it displays the values you are searching for. Very handy, if you want to find the correct value for the `TZ` variable.

Basic usage:
```
tzselect
```

Output - first question:
```
Please identify a location so that time zone rules can be set correctly.
Please select a continent, ocean, "coord", or "TZ".
 1) Africa
 2) Americas
 3) Antarctica
 4) Asia
 5) Atlantic Ocean
 6) Australia
 7) Europe
 8) Indian Ocean
 9) Pacific Ocean
10) coord - I want to use geographical coordinates.
11) TZ - I want to specify the timezone using the Posix TZ format.
#? █
```

Complete output example for Germany:
```
Please identify a location so that time zone rules can be set correctly.
Please select a continent, ocean, "coord", or "TZ".
 1) Africa
 2) Americas
 3) Antarctica
 4) Asia
 5) Atlantic Ocean
 6) Australia
 7) Europe
 8) Indian Ocean
 9) Pacific Ocean
10) coord - I want to use geographical coordinates.
11) TZ - I want to specify the timezone using the Posix TZ format.
#? 7
Please select a country whose clocks agree with yours.
 1) Albania               14) France                27) Luxembourg            40) Serbia
 2) Andorra               15) Germany               28) Malta                 41) Slovakia
 3) Austria               16) Gibraltar             29) Moldova               42) Slovenia
 4) Belarus               17) Greece                30) Monaco                43) Spain
 5) Belgium               18) Guernsey              31) Montenegro            44) Svalbard & Jan Mayen
 6) Bosnia & Herzegovina  19) Hungary               32) Netherlands           45) Sweden
 7) Britain (UK)          20) Ireland               33) North Macedonia       46) Switzerland
 8) Bulgaria              21) Isle of Man           34) Norway                47) Turkey
 9) Croatia               22) Italy                 35) Poland                48) Ukraine
10) Czech Republic        23) Jersey                36) Portugal              49) Vatican City
11) Denmark               24) Latvia                37) Romania               50) Åland Islands
12) Estonia               25) Liechtenstein         38) Russia
13) Finland               26) Lithuania             39) San Marino
#? 15
Please select one of the following timezones.
1) Büsingen
2) most of Germany
#? 2

The following information has been given:

        Germany
        most of Germany

Therefore TZ='Europe/Berlin' will be used.
Selected time is now:   Mon Jun 10 10:59:14 CEST 2024.
Universal Time is now:  Mon Jun 10 08:59:14 UTC 2024.
Is the above information OK?
1) Yes
2) No
#? 1

You can make this change permanent for yourself by appending the line
        TZ='Europe/Berlin'; export TZ
to the file '.profile' in your home directory; then log out and log in again.

Here is that TZ value again, this time on standard output so that you
can use the /usr/bin/tzselect command in shell scripts:
Europe/Berlin
```
