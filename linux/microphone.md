# Echo test: hearing the microphone

Source: [Debian.org - PulseAudio](https://wiki.debian.org/PulseAudio#Echo_test:_hearing_the_microphone)

<br>

Start mic echo test:  
```shell
pactl load-module module-loopback
```

<br>

Stop mic echo test:  
```shell
pactl unload-module module-loopback
```