Sublime Better Javascript
=========================

This package is an improved version of Sublime Text's JavaScript package, focused mainly on improving symbol navigation.

The default JavaScript language definition in Sublime Text has *quirky* symbol identification, and fills your symbols list with noise like anonymous functions, object instantiation, and even calls to console.log().

![Useless Symbol List](http://int3h.github.io/sublime-better-javascript/images/screenshot-bad-symbols.png)

This project fixes Sublime so only named function definitions and function prototype attributes show up in the symbol list. It also fixes how these symbols are displayed in the symbol list, so that only the function names are shown.

![Improved Symbol List](http://int3h.github.io/sublime-better-javascript/images/screenshot-good-symbols.png)

This package has been tested on Sublime Text 2 and 3.


Installing
--------------

Install via [package control](http://wbond.net/sublime_packages/package_control) (search for 'Better JavaScript').

Alternatively, `cd` to you Sublime packages directory, then:

    git clone git@github.com:int3h/sublime-better-javascript.git 'Sublime Better JavaScript'


### Refreshing the symbols of existing files

In some cases, especially with Sublime Text 3, the symbols list of files that have been previously opened may not refresh with the improved symbols.

The most reliable way I've found to fix this is to close all open JavaScript files and quit Sublime (it's important that all JS files are closed when Sublime exits.) Then delete the `Index` and `Cache` subdirectories in your Sublime user data directory (the parent of the `Packages` directory.) In Mac OS X, I've also had to delete ~/Library/Caches/com.sublimetext.3 (or com.sublimetext.2).


Uninstalling
------------
Before uninstalling Better JavaScript, be sure close all open JavaScript/JSON
files, and remove "JavaScript" from the "ignored_packages" list in your Sublime
user settings. You can now safely uninstall the Better JavaScript package.

It's normal to see a few errors from Sublime about not being able to find syntax files. Just exit
Sublime and re-open it, and everything should be fixed.


Details
-------

The default JavaScript.tmLanguage makes some... odd decisions on what namespace to put certain tokens in. Specifically, it puts a lot of tokens in the entity.name.* namespaces that probably shouldn't be.

There is a file, `Symbol List Banned.tmPreferences`, in the JavaScript package which ostensibly is supposed to filter out some specific sub-namespaces from the symbols list, but it doesn't really work. I modified `Symbol List Banned.tmPreferences` to specifically match object instantiation and console.log calls, and set them to not show up in the symbol list.

I also modified `Symbol List Function.tmPreferences` to do additional processing of function names with regexes, to strip out extraneous characters (like "function" and "= function()") before showing them in the symbol list. An additional file, `Symbol List Prototype.tmPreferences`, was added to include function prototype attributes in the symbol list.
