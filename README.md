# Solarized for Windows Console
This is the Solarized theme for Windows Console.

See [Solarized Official Homepage](http://ethanschoonover.com/solarized) for
more infomation.

## Installation
Download `solarized-dark.reg` or `solarized-light.reg` and double-click to
import.

## Colors
Windows Consolse uses a different color map from \*nix's, but they can be
easily mapped to each other:

```
NR  CMD           POWERSHELL   TERMCOL/16    SOLARIZED
--  ------------  -----------  ------------  ---------
 0  Black         Black         0 black      base02
 1  Blue          DarkBlue      4 blue       blue
 2  Green         DarkGreen     2 green      green
 3  Aqua          DarkCyan      6 cyan       cyan
 4  Red           DarkRed       1 red        red
 5  Purple        DarkMagenta   5 magenta    magenta
 6  Yellow        DarkYellow    3 yellow     yellow
 7  White         Gray          7 white      base2
 8  Gray          DarkGray      8 brblack    base03
 9  Light Blue    Blue         12 brblue     base0
10  Light Green   Green        10 brgreen    base01
11  Light Aqua    Cyan         14 brcyan     base1
12  Light Red     Red           9 brred      orange
13  Light Purple  Magenta      13 brmagenta  violet
14  Light Yellow  Yellow       11 bryellow   base00
15  Bright White  White        15 brwhite    base3
```

## Vim
To show Solarized colors correctly in vim, you need to apply
[this patch](https://github.com/altercation/vim-colors-solarized/pull/150) to
the Vim Solarized colorscheme, or use
[my fork](https://github.com/brglng/vim-colors-solarized) instead.

In addition, if you want
[vim-airline](https://github.com/vim-airline/vim-airline) to show Solarized
colors correctly, you need to apply this patch to `vim-airline`:

(I have not issued a PR to `vim-airline` because my PR to Solarized isn't
merged. It looks like the author of Solarized has stopped maintaining it.)

```diff
diff --git a/autoload/airline/highlighter.vim b/autoload/airline/highlighter.vim
index 0d3dce9..a175b68 100644
--- a/autoload/airline/highlighter.vim
+++ b/autoload/airline/highlighter.vim
@@ -6,9 +6,9 @@ let s:is_win32term = (has('win32') || has('win64')) && !has('gui_running') && (e
 let s:separators = {}
 let s:accents = {}
 
-function! s:gui2cui(rgb, fallback)
-  if a:rgb == ''
-    return a:fallback
+function! s:gui2cui(rgb, termcol)
+  if a:rgb == '' || (a:termcol != '' && a:termcol >= 0 && a:termcol <= 15)
+    return a:termcol
   endif
   let rgb = map(matchlist(a:rgb, '#\(..\)\(..\)\(..\)')[1:3], '0 + ("0x".v:val)')
   let rgb = [rgb[0] > 127 ? 4 : 0, rgb[1] > 127 ? 2 : 0, rgb[2] > 127 ? 1 : 0]
diff --git a/autoload/airline/themes/solarized.vim b/autoload/airline/themes/solarized.vim
index 30ba47e..0ffd1d5 100644
--- a/autoload/airline/themes/solarized.vim
+++ b/autoload/airline/themes/solarized.vim
@@ -7,27 +7,28 @@ function! airline#themes#solarized#refresh()
   let s:background  = get(g:, 'airline_solarized_bg', &background)
   let s:ansi_colors = get(g:, 'solarized_termcolors', 16) != 256 && &t_Co >= 16 ? 1 : 0
   let s:tty         = &t_Co == 8
+  let s:win32       = (has('win32') || has('win64')) ? 1 : 0
 
   """"""""""""""""""""""""""""""""""""""""""""""""
   " Colors
   """"""""""""""""""""""""""""""""""""""""""""""""
   " Base colors
-  let s:base03  = {'t': s:ansi_colors ?   8 : (s:tty ? '0' : 234), 'g': '#002b36'}
-  let s:base02  = {'t': s:ansi_colors ? '0' : (s:tty ? '0' : 235), 'g': '#073642'}
-  let s:base01  = {'t': s:ansi_colors ?  10 : (s:tty ? '0' : 240), 'g': '#586e75'}
-  let s:base00  = {'t': s:ansi_colors ?  11 : (s:tty ? '7' : 241), 'g': '#657b83'}
-  let s:base0   = {'t': s:ansi_colors ?  12 : (s:tty ? '7' : 244), 'g': '#839496'}
-  let s:base1   = {'t': s:ansi_colors ?  14 : (s:tty ? '7' : 245), 'g': '#93a1a1'}
-  let s:base2   = {'t': s:ansi_colors ?   7 : (s:tty ? '7' : 254), 'g': '#eee8d5'}
-  let s:base3   = {'t': s:ansi_colors ?  15 : (s:tty ? '7' : 230), 'g': '#fdf6e3'}
-  let s:yellow  = {'t': s:ansi_colors ?   3 : (s:tty ? '3' : 136), 'g': '#b58900'}
-  let s:orange  = {'t': s:ansi_colors ?   9 : (s:tty ? '1' : 166), 'g': '#cb4b16'}
-  let s:red     = {'t': s:ansi_colors ?   1 : (s:tty ? '1' : 160), 'g': '#dc322f'}
-  let s:magenta = {'t': s:ansi_colors ?   5 : (s:tty ? '5' : 125), 'g': '#d33682'}
-  let s:violet  = {'t': s:ansi_colors ?  13 : (s:tty ? '5' : 61 ), 'g': '#6c71c4'}
-  let s:blue    = {'t': s:ansi_colors ?   4 : (s:tty ? '4' : 33 ), 'g': '#268bd2'}
-  let s:cyan    = {'t': s:ansi_colors ?   6 : (s:tty ? '6' : 37 ), 'g': '#2aa198'}
-  let s:green   = {'t': s:ansi_colors ?   2 : (s:tty ? '2' : 64 ), 'g': '#859900'}
+  let s:base03  = {'t': s:ansi_colors ?  (s:win32 ?  8 :  8) : (s:tty ? '0' : 234), 'g': '#002b36'}
+  let s:base02  = {'t': s:ansi_colors ?  (s:win32 ?  0 :  0) : (s:tty ? '0' : 235), 'g': '#073642'}
+  let s:base01  = {'t': s:ansi_colors ?  (s:win32 ? 10 : 10) : (s:tty ? '0' : 240), 'g': '#586e75'}
+  let s:base00  = {'t': s:ansi_colors ?  (s:win32 ? 14 : 11) : (s:tty ? '7' : 241), 'g': '#657b83'}
+  let s:base0   = {'t': s:ansi_colors ?  (s:win32 ?  9 : 12) : (s:tty ? '7' : 244), 'g': '#839496'}
+  let s:base1   = {'t': s:ansi_colors ?  (s:win32 ? 11 : 14) : (s:tty ? '7' : 245), 'g': '#93a1a1'}
+  let s:base2   = {'t': s:ansi_colors ?  (s:win32 ?  7 :  7) : (s:tty ? '7' : 254), 'g': '#eee8d5'}
+  let s:base3   = {'t': s:ansi_colors ?  (s:win32 ? 15 : 15) : (s:tty ? '7' : 230), 'g': '#fdf6e3'}
+  let s:yellow  = {'t': s:ansi_colors ?  (s:win32 ?  6 :  3) : (s:tty ? '3' : 136), 'g': '#b58900'}
+  let s:orange  = {'t': s:ansi_colors ?  (s:win32 ? 12 :  9) : (s:tty ? '1' : 166), 'g': '#cb4b16'}
+  let s:red     = {'t': s:ansi_colors ?  (s:win32 ?  4 :  1) : (s:tty ? '1' : 160), 'g': '#dc322f'}
+  let s:magenta = {'t': s:ansi_colors ?  (s:win32 ?  5 :  5) : (s:tty ? '5' : 125), 'g': '#d33682'}
+  let s:violet  = {'t': s:ansi_colors ?  (s:win32 ? 13 : 13) : (s:tty ? '5' : 61 ), 'g': '#6c71c4'}
+  let s:blue    = {'t': s:ansi_colors ?  (s:win32 ?  1 :  4) : (s:tty ? '4' : 33 ), 'g': '#268bd2'}
+  let s:cyan    = {'t': s:ansi_colors ?  (s:win32 ?  3 :  6) : (s:tty ? '6' : 37 ), 'g': '#2aa198'}
+  let s:green   = {'t': s:ansi_colors ?  (s:win32 ?  2 :  2) : (s:tty ? '2' : 64 ), 'g': '#859900'}
 
   """"""""""""""""""""""""""""""""""""""""""""""""
   " Simple mappings
```
