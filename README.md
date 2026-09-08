# BASH 2.0

Note:
My version of bash is more geared toward collecting extra information and making things more presentable for the competitive strafing community. This version of bash has no new or advanced detection methods or anything of that nature, the only new auto bans added are for certain gain logs. If you want to ban users using null movement scripts, this version simply will not do it.

## If you're already using Bash2 on your server, you will need to delete your current cfg(or rename it to something like bash.cfg.bak) and let it regenerate. This version is much more configurable.

Main Changes:
* Remove useless code that was intended for the older movement community (nulls detections, faking if a player is on the ground to brick optis, etc)
* Add configurations for dev and identical strafes bans
* Add auto bans for certain ridiculous gain logs
* Add configurations for a steam group that's whitelisted from the auto bans
* Logs are more presentable/searchable

Changes in this fork:
* **Deviation HUD** - puts the rolling average and deviation of your last 50 start and end strafe offsets on screen, so you can see the numbers the detections act on while you play instead of only afterwards in the logs. The menu is found in `sm_bash` under "live deviation HUD"
* Start and end offsets are also tracked across a whole run and printed on finish, since the 50-sample window the detections use is too short to judge a run by
* Reset strafe tracking on style change, so an autostrafe style's identical offsets are no longer counted against the style you switch to
* Don't count gain on ticks with no wish input, and fix a stationary check that only matched exactly 0.0 and so scored standing still as a perfect strafe
* Only accumulate gain on ticks that actually gained
* `bash_dev_ignore_styles` - style ids exempt from the dev and identical-strafe detections. Unlike `bash_bypass` this keeps gain logging on, so an autostrafe style can still be watched
* `bash_report_ignore_styles` - style ids whose detections are not passed to the `Bash_OnDetection` forward. Chat, mainly used to stop autosync logs being sent to sourcejump.
## Commands

```
sm_bash or sm_bash2 - open the bash settings menu
sm_devhud or sm_bashhud - open the deviation HUD menu (mode, position, run offsets on finish)
bash2_stats <name> - Show strafe stats
bash2_admin - toggle admin mode, lets you enable/disable printing of bash logs into the chat.
bash2_personal - toggle personal mode, lets you enable/disable only seeing your own logs in chat.
bash2_test  - trigger a test message so you can know if webhooks are working
```

## Depencenies for the Discord Messages

* [SteamWorks](https://forums.alliedmods.net/showthread.php?t=229556)
* [sm-json](https://github.com/clugg/sm-json) (only for compiling)

If you compile it yourself and the discord embeds stop sending, that's sm-json sizing nested objects. `json_encode_size` in `json.inc` uses a fixed worst case for a child object instead of measuring it, and an embed nests several, so it overruns the heap and the message is dropped with no obvious error. Two fixes, either works:

* in `json.inc`, `cell_length = child != null ? max_size : JSON_NULL_BUFFER_SIZE;` becomes `cell_length = child != null ? json_encode_size(child, options) : JSON_NULL_BUFFER_SIZE;`
* or `#pragma dynamic 1234567` at the top of `shavit-bash2.sp`, which leaves the include alone

## Anticheat bypass

If you are using shavit bhoptimer, you can add "bash_bypass" into a style's special string to disable detection for this style.
