Sublime Better Javascript
=========================

This is a collection of tweaks to Sublime Text's JavaScript file handling, focused mainly on improving symbol navigation.

The default JavaScript language definition in Sublime Text considers anonymous functions, object instantiation like `new Foo()`, and `console.log()` calls to be symbols, causing your "Goto Symbol..." menu to become cluttered and useless.

This project fixes Sublime so only named function definitions and function prototype attributes show up in the symbol list. It also fixes how these symbols are displayed in the symbol list, so that only the function name is shown, so `function Foo()` becomes just `Foo`, `Bar.foo = function()` becomes `Bar.foo`, and so on. 

These tweaks have been tested on Sublime Text 2 and 3.


Installing
--------------

`cd` to your Sublime Text packages directory (you can find it via the "Preferences -> Browse Packages..." menu item; with ST3 on Mac OS X, this is `~/Library/Application\ Support/Sublime\ Text\ 3/Packages/`, for example.) Then checkout this repo into a folder named `JavaScript`:

    git clone https://github.com/int3h/sublime-better-javascript.git JavaScript

Alternatively, you can manually create a `JavaScript` subdirectory in your packages directory, and [download](https://github.com/int3h/sublime-better-javascript/archive/master.zip) and extract the contents of this repo to that folder.


### Refreshing the symbols of existing files

In some cases, especially with Sublime Text 3, the symbols list of files that have been previously opened may not refresh with the improved symbols. 

The most reliable way I've found to fix this is to close all open JavaScript files and quit Sublime (it's important that all JS files are closed when Sublime exits.) Then delete the `Index` and `Cache` subdirectories in your Sublime user data directory (the parent of the `Packages` directory.) In Mac OS X, I've also had to delete ~/Library/Caches/com.sublimetext.3 (or com.sublimetext.2). 


Details
-------

The default JavaScript.tmLanguage makes some... odd decisions on what namespace to put certain tokens in. Specifically, it puts a lot of tokens in the entity.name.* namespaces that probably shouldn't be. 

There is a file, `Symbol List Banned.tmPreferences`, in the JavaScript package which ostensibly is supposed to filter out some specific sub-namespaces from the symbols list, but it doesn't really work. I modified `Symbol List Banned.tmPreferences` to specifically match object instantiation and console.log calls, and set them to not show up in the symbol list.

I also modified `Symbol List Function.tmPreferences` to do additional processing of function names with regexes, to strip out extraneous characters (like "function" and "= function()") before showing them in the symbol list. An additional file, `Symbol List Prototype.tmPreferences`, was added to include function prototype attributes in the symbol list.

To get Sublime to use these updated files instead of its built-in, we have to place them in the user Packages folder with the same package and file name as the built-in versions. That's why the package folder has to be named "JavaScript" exactly, and the files have the names that they do. It's also how we get away with only distributing the files we changed, rather than the entire language definition: the files missing here will just cause Sublime to use its built-in version.


Questions
------

* **Can you put this on [package control](http://wbond.net/sublime_packages/package_control)?** No, unfortunately. It's not enough to add our own language settings to Sublime; we need to also disable the existing language settings. 
  
  To my knowledge, the only way to do that is to override the built-in files with your own version, by giving the package the same name as the built-in version, "JavaScript". If we wanted to distribute this via Package Control, it would have to retain that package name, which is undesirable for multiple reasons.

  There may be a way to do it via distributing an entirely new language definition, with new scope names and so on, but it'd be messy. If you have an idea on a better way to do it, let me know.

* **Other issues?** Feel free to open up an issue here if you're having trouble, or have ideas for improvements. I am also very receptive to pull requests.
