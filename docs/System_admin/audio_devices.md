# OBS add virtual microphone
- [OBS ad virtual microphone](https://obsproject.com/forum/resources/obs-ad-virtual-microphone.539/)
- [OBS ad virtual microphone](https://obsproject.com/forum/threads/quick-and-easy-virtual-microphone-for-linux.158340/)

```bash
pactl load-module module-null-sink media.class=Audio/Sink sink_name=Virtual-Mic channel_map=front-left,front-right
pactl load-module module-null-sink media.class=Audio/Source/Virtual sink_name=Virtual-Mic channel_map=front-left,front-right
pw-link Virtual-Mic:monitor_FL Virtual-Mic:input_FL
pw-link Virtual-Mic:monitor_FR Virtual-Mic:input_FR
```

