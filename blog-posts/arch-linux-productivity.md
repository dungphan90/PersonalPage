I'm very happy to come back after a long year. I hope to keep this blog going for longer this time. For this comeback writing, I would like to introduce a few productivity softwares that served me very well recently. 

## SpaceVIM


## VIMium 
VIMium is an open-source `google-chrome` extension that allows you to navigate the web browser with Vim key bindings. It turns `google-chrome` into a keyboard-driven software. For a long time, web browser and PDF viewer are the only two reasons I keep a mouse on my desk. It's no longer the case as I found VIMium and Zathura (a VIM-like document viewer). Now, there is a small number of websites that VIMium does not (fully) support. This issue can be easily overcome if you know the *normal* chrome key bindings (like `Alt + TAB_NUMBER`).

VIMium can be download from [Chrome Web Store]() or from the author's `github`. There are not much to config to just use the extension. The author did a good job in transfering the Vim key bindings and make them work for a web browser. Of course, there are so much flexibility if you want to tailor the keybinds to your liking, but I find I live well with what VIMium deliver by default.

The keybinds that I used the most with VIMium are
1. `f/F` to view all the links. If you type the follow-up letters in lowercase (no `Shift +`) then the corresponding link will be opened in the current tab, otherwise (`Shift + FOLLOW_UP`) it will be opened in a new tab, right next to the current tab.
2. `o/O` to search keywords or go to a URL. Again, just like `f/F`, the lowercase `o` will open the new website in the current tab, while the uppercase `O` will open a new tab.
3. `j/k` and `d/u` to navigate the website vertically. `j/k` does line-by-line while `d/u` goes by half of the view. Just like Vim, `gg` to go up to the very beginning of the website and `G` to get to the bottom of it.
4. For tab control, `t` to open new tab. `x` to close the current tab. `T` search through the open tabs, `J/K` to move to the tab on the left/right respectively.
5. For search, `/` to enter search mode, `n/N` to move forward/backward among matches.

Those are basically all you need to browse the Web. If you need advanced nagivating, copy/paste features, you can always toggle the `help` pop-up by `?`. Everything is exactly like Vim. I haven't even to try out the advanced options that VIMium offers. I will update this if I find new, interesting feature.

## Tiling window manager
Tiling window managers are great tool to save up your monitor real estate as well as gearing toward a keyboard-driven experience. My choice is `AwesomeWM`. Its configuration is written in `lua` but even if you're never heard of `lua`, it's simple enough for you to start tweaking with a few Google searches. There are a lot of themes to choose from and it's quite easy to customize them to your preferences. Key bindings are a breaze to set-up or change and they come with built-in documentation/user-manual that you can toggle in a keys stroke. I think this is one of the best features of `AwesomeWM`, it helps new users learn the ways around `Awesome` very quickly.

