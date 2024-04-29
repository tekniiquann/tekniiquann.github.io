---
layout: post
title: "GNU Emacs Configuration"
categories_short_name: cc++_compilers
meta: "cc++compilers"
---

Though [Emacs](https://en.wikipedia.org/wiki/Emacs) family is considered as ancient and slowly dying-out text editors and programming softwares, the [GNU Emacs](https://en.wikipedia.org/wiki/GNU_Emacs) is still daily driving coding platform of plenty of users. I'm one of them, and [GNU Emacs](https://en.wikipedia.org/wiki/GNU_Emacs) is NOT some sort of unconvenient browser for me. 
Emacs is one of two default supported editors on HPC cluster (another on is [vi/vim](https://ex-vi.sourceforge.net/)). **It is code editor with handy key-combinations, supports shell-like text navigation and has plenty extendible packages (similar to VScode plugins)**. However, most of these come only after simply and necessary configuration.

GNU Emacs configuration could easily turns into hardcore coding thanks for the [ELisp](https://en.wikipedia.org/wiki/Emacs_Lisp) --- the driving language of GNU Emacs's extensibility and dialect of functional programming language [Lisp](https://en.wikipedia.org/wiki/Lisp_(programming_language)). This small article will not get into this rabbit hole. Now let's get start!

For Debian-based OS, `apt install emacs` is natural way for installation, while on BSD e.g., FreeBSD, `pkg install` is corresponding routine. If you want be geeky, compiling and installing GNU Emacs from source code is a good option, here is the [GNU Emacs project page](https://savannah.gnu.org/projects/emacs/).

After installation, run `ls -al ~` WITHOUT `su` privilege to check if there is a directory or file named as `.emacs.d` or `.emacs`. If you have a `.emacs.d` directory like my desktop, then you could run `touch .emacs.d/init.el` to create the configuration file. After then, using your preferred text editor to open it. Because `.emacs.d` folder is hidden, the most easy way to open it is using `vi .emacs.d/init.el`.

The next will be adding ELisp commands and functions to turn off the GNU starting screen and to turn on global line number:
```text
;; turn off the startup screen
(setq inhibit-startup-screen t)

;; bell blinking
(setq visible-bell t)

;; Display line numbers in every buffer
(global-display-line-numbers-mode 1)
``` 
Most of programmers want change the default theme, which has plain white color, to something more soft to our eyes. To achieve this, one needs to load different theme packages than the default theme. The hard way to do this coding from scratch with [ELisp](https://en.wikipedia.org/wiki/Emacs_Lisp), while the easy way is install theme packages from [Elpa](https://elpa.gnu.org/) and [Melpa](https://melpa.org/#/) packages achieves. We use easy way at here, adding the following codes into your `init.el`:
```text
(require 'package)
(add-to-list 'package-archives '("gnu" . "https://elpa.gnu.org/packages/"))
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/"))
```
This is all necessary setting for installing theme packages, you should launch Emacs after saving.
**If things went well so far, you should notice that the GNU stating pages has gone**, but the GUI is still plain white. We should install other theme now such as `ef-theme`! Using the key comb `M-x`, where `M` may be `Alt` on your keyboard, then tap in `package-install` in `minibuffer` and tap `RET` key. `minibuffer` then waits you to tap in `ef-theme` and tap `RET` to install the theme. It may take one minute to install. 

The installed packages will not automatically launched until you add loading command:
```text
(load-theme 'ef-night :no-confirm)
```
into `.emacs.d/init.el`. For more usage of , please check [EmacsWiki:elpa](https://www.emacswiki.org/emacs/ELPA) and [EmacsWiki:Melpa](https://www.emacswiki.org/emacs/MELPA).

The following function implements the **bracket** matching in GNU Emacs. This basic feature is missing from plain installation.
```text
(global-set-key "%" 'match-paren)

(defun match-paren (arg)
  "Go to the matching paren if on a paren; otherwise insert %."
  (interactive "p")
  (cond ((looking-at "\\s(") (forward-list 1) (backward-char 1))
        ((looking-at "\\s)") (forward-char 1) (backward-list 1))
        (t (self-insert-command (or arg 1)))))

;; show matched parenthesis
(show-paren-mode 1)
```
At this stage, our simple and necessary configuration of GNU Emacs has done. Restarting emacs, one should get a nicely colorized emacs. All the configurations motioned above work in same way both on HPC and desktop environments. 

- last time edited @31st. Jan. 2024
