## TODO

### Feat

* Allow turning off the message **"Invalid format. Please ensure your test follows the required pattern."** so it can be used as an ad-hoc regex viewer.

### Feat

* Add a VS Code command so the user can hit `Ctrl+R, E` in any file and it opens a new **RegexMatch View** with that text.

  * We can add two commands:

    * one that opens the regex for simple syntax highlighting (possibly side-by-side), and
    * one that opens it with the test area.

#### Or better:

* Add a command that, for selected text in any file, opens a side panel with:

  * a preview of the regex with syntax highlighting (useful everywhere except JS `//g` regexes, which are already highlighted well), and
  * an input test panel.
  
The side view would allow not only previewing and providing inputs, but also editing the regex, with the selected regex in the original file updating automatically as you edit in the side panel.

### Feat

* Allow switching the regex engine to **Oniguruma**.
To add this we’ll need either this: [https://github.com/slevithan/oniguruma-to-es](https://github.com/slevithan/oniguruma-to-es)
or this: [https://github.com/microsoft/vscode-oniguruma](https://github.com/microsoft/vscode-oniguruma).

Also see https://github.com/firasdib/Regex101/issues/2297#issuecomment-2564604681.

  * This will allow comments and the `(?x)` flag.

### Feat

* In the mode above, we write regexes without `/.../` boundaries. But we should also allow omitting them in JS mode for convenience.

  * In that case, we probably want a “default flags” setting in `settings.json`.
  * In this `/.../`-less JS mode, we keep the ability to write normal JS regexes like `/.../g` if needed (no forced escaping of `/`!).

### Feat

* Allow comment styles like `(?# ...)` and `[0-9] # comment` even in JS mode for convenience.

  * Don’t forget to allow escaping them with `\(\?#` or `\#` (depending on what exactly you parse), and use them instead of the current `---` to separate the test area.
  * Use a special prefix like `(?#---` to distinguish it from normal comments that are meant to be part of Oniguruma inside the regex. Like this:

```
[a-z 0-9]+
(?#---
your regex test input
---)
[a-z 0-9]+
(?#
but this is a real comment, part of Oniguruma
)
```

### Feat

* Allow multiple independent test blocks that use the same (previous) regex.
  Example:

```
[a-z 0-9]+ (?# regex #1 )
(?#---
test input 1
---)
(?#---
test input 2 for the same regex above
---)
[a-z 0-9]+ (?# regex #2 )
(?#---
test input 1 for regex 2
---)
```


### Feat

Add a keyword that will allow the test input to run to the end of the file without closing the test area:

```txt
[a-z 0-9]+
(?#<<<EOF
test input goes to the end of the file; any `---)` sequences that would normally signify the end are ignored and become part of the input
```
