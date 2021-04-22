Screw you gnome-terminal!
===============

Posted: 2009-07-27 09:37:54
-------------------------

OK, geekiness time. Most people hate a blinking cursor in a terminal - that huge blinking square is irritating beyond belief. Strangely, though, a blinking caret (the thin line you see in most text editors) is quite all right by most people. However, in their infinite wisdom, GNOME decided that all users want the caret and cursor to behave the same, and have hard-wired the cursor behavior to be the same as the caret behavior, which is stupid. At least they did for a few versions; they are now decoupled again, but they don't give you an easy way to turn off the blinking. Here is the magic incantation to access the secret setting to turn off blinking:

<code>gconftool-2 -s /apps/gnome-terminal/profiles/Default/cursor_blink_mode -t string off</code>

Done! now you can hack in peace. :)
