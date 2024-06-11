# Language related stuff
# Change system language in Debian
Sources: 
- [wiki.debian.org](https://wiki.debian.org/ChangeLanguage)
- [baeldung.com - Locale Environment Variables in Linux](https://www.baeldung.com/linux/locale-environment-variables)

\1. Check the current language:
```
:~$ env | grep LANG
LANG=de_DE.UTF-8
### or
:~$ locale
```

Structure of the LANG Variable: `<two letter language code>_<two letter country code>.<Character-Set>`

\2. Identify the LANG code you want to change to.

- List of two letter language codes: [wikipedia.org - List of ISO 639 language codes](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes)
- List of two letter country codes: [wikipedia.org - ISO 3166-1](https://en.wikipedia.org/wiki/ISO_3166-1)
- The only Character set I ran into so far, is `UTF-8`. However you can check out the [man-page](https://www.man7.org/linux/man-pages/man7/charsets.7.html) (`man 7 charsets`) for further details.
- Bonus-Link - List of time zones: [wikipedia.org - List of tz database time zones](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones)

All available combinations can be found in: `/usr/share/i18n/SUPPORTED`

Lets search for en_US:
```
:~$ grep -i en_us /usr/share/i18n/SUPPORTED
en_US.UTF-8 UTF-8
en_US ISO-8859-1
en_US.ISO-8859-15 ISO-8859-15
```

Gotcha: `en_US.UTF-8`

\3. Export the new variable (root needed - `sudo -i` e.g.):
```
:~# export LANG=en_US.UTF-8
```

\4. Reconfigure locale (root needed):
```
:~# dpkg-reconfigure locales
```
_Follow the steps on the display._

\5. Reboot is needed for the changes to take effect:
```
:~# reboot
```

⚠ OBACHT

I changed only the System language, but left most of the other counrty/language specific formats as before:
```
:~$ locale
LANG=en_US.UTF-8
LANGUAGE=
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC=de_DE.UTF-8
LC_TIME=de_DE.UTF-8
LC_COLLATE="en_US.UTF-8"
LC_MONETARY=de_DE.UTF-8
LC_MESSAGES="en_US.UTF-8"
LC_PAPER=de_DE.UTF-8
LC_NAME=de_DE.UTF-8
LC_ADDRESS=de_DE.UTF-8
LC_TELEPHONE=de_DE.UTF-8
LC_MEASUREMENT=de_DE.UTF-8
LC_IDENTIFICATION=de_DE.UTF-8
LC_ALL=
```
So the output of `date` is still German:
```
:~$ date
Di 9. Apr 17:17:49 CEST 2024
```


# Troubleshooting

## ERROR message: `Setting locale failed`
```
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
        LANGUAGE = (unset),
        LC_ALL = (unset),
        LC_ADDRESS = "de_DE.UTF-8",
        LC_NAME = "de_DE.UTF-8",
        LC_MONETARY = "de_DE.UTF-8",
        LC_PAPER = "de_DE.UTF-8",
        LC_IDENTIFICATION = "de_DE.UTF-8",
        LC_TELEPHONE = "de_DE.UTF-8",
        LC_MEASUREMENT = "de_DE.UTF-8",
        LC_TIME = "de_DE.UTF-8",
        LC_NUMERIC = "de_DE.UTF-8",
        LANG = "en_US.UTF-8"
    are supported and installed on your system.
perl: warning: Falling back to a fallback locale ("en_US.UTF-8").
```

In my case `de_DE.UTF-8` was not properly installed. Reconfiguring locales and selecting both laguages `en_US.UTF-8` and `de_DE.UTF-8` solved the issue:

```
dpkg-reconfigure locales
```

Output of `locale` after reconfiguration:
```
:~# locale
LANG=en_US.UTF-8
LANGUAGE=
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC=de_DE.UTF-8
LC_TIME=de_DE.UTF-8
LC_COLLATE="en_US.UTF-8"
LC_MONETARY=de_DE.UTF-8
LC_MESSAGES="en_US.UTF-8"
LC_PAPER=de_DE.UTF-8
LC_NAME=de_DE.UTF-8
LC_ADDRESS=de_DE.UTF-8
LC_TELEPHONE=de_DE.UTF-8
LC_MEASUREMENT=de_DE.UTF-8
LC_IDENTIFICATION=de_DE.UTF-8
LC_ALL=
```