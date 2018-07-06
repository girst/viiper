# VIIper - a Snake Clone for Unicode-compatible Terminals

TODO: find a better name

## Dependencies

You'll need either a terminal emulator with good Unicode (Emoji) support and a
compatible fonts, or an actual DEC VT220 to fully enjoy the graphics of this
game. This is what I'd recommend:

 - A VTE based terminal (like GNOME Terminal and a whole bunch of others)
 - Google Noto's Color Emoji Font (Fedora: `google-noto-emoji-color-fonts.noarch`)
 - DejaVu Sans Mono with Braille characters patched in (fetch from Ubuntu)

## Keybindings

`h`, `j`, `k`, `l` or cursor keys move the snake. 
`r` to restart, `p` to pause, `q` to quit. 

## TODO

 - DONE ~~base game: fixed field size, fixed speed~~
 - DONE ~~food (unicode)~~
 - DONE ~~put 'sprites' into `schemes.h`~~
 - DONE ~~snake elongates~~
 - DONE ~~unicode chars~~
 - DONE ~~input buffer (so fast 180° turns get executed)~~
 - DONE ~~only redraw changing parts of the screen~~
 - DONE ~~input out of whack when stopping (^Z) and resuming~~
 - DONE ~~keybindings for restart, pause, redraw~~
 - DONE ~~on dying: show end screen, allow restarting~~
 - DONE ~~score, increasing speed, ~~timer~~~~
 - DONE ~~bonus/special items: slower snake, shorter snake, etc.~~
 - TODO decaying points? (more points the faster you get the food)
 - TODO wall-wrap-around mode?
 - TODO fix all `grep -n TODO viiper.c`
 - TODO VT220 chrsaet based on soft downloadable character set (->minesviiper)

## Notes

### terminal compat

to display emojis, we need a terminal that can handle a color emoji font (no
shit, sherlock). mlterm, xterm and urxvt didn't work in my tests (mlterm might
work if compiled correctly, the other two use bitmap fonts and i don't think
there are any w/ emoji support). 
i intend to put bonus items in the game that will only be visible for a short
time. when they get near the end of their life, SGI-5 (blink) will make them
blink. this is supported in gnome-term 3.28 (vte 0.52) which is supplied with
fedora 28. [bug report](https://bugzilla.gnome.org/show_bug.cgi?id=579964)
for KDE's konsole you'll need [this
fontconfig](https://gist.github.com/IgnoredAmbience/7c99b6cf9a8b73c9312a71d1209d9bbb)
and follow the steps inside.


### strange behaviour of SIGALRM

I'm using SIGALRM to advance the snake's position. during some refactoring I
noticed that when the signal handler returns, a STX (ASCII 0x02) byte gets
pushed onto stdin.

### DejaVu Sans Mono: Braille Characters

This font (while otherwise beautiful) does not by default include glyphs for the
braille characters in Unicode. And gnome-terminal falls back to some very ugly
rendition. 

Ubuntu patches this font to include those glyphs, so you can just fetch it from
there, or patch the font yourself. For this, open
`/usr/share/fonts/dejavu/DejaVuSansMono.ttf` and `DejaVuSans.ttf` and copy the
braille section to the Mono variant. You can save the font under a different
name in the same directory and the fallback will then work correctly. 
