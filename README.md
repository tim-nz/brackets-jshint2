# brackets-jshint2

A Brackets extension to enable [JSHint](https://jshint.com/) linting. This is a fork of [Raymond Camden's original brackets-jshint extension](https://github.com/cfjedimaster/brackets-jshint), which hasn't been updated since 2016 and still uses version 2.9.4 of JSHint. I haven't done anything beyond updating JSHint, the metadata, and the instructions.

## Installation

To install, download all files and place them in a new folder called `tim-nz.jshint2` in the [Brackets user extensions folder](https://github.com/brackets-cont/brackets/wiki/Extension-Locations#user-extensions-folder), then press F5 to reload Brackets.

You'll probably want to turn off the outdated version of JSLint that comes with Brackets, and possibly ESLint too. To do that, go to the Extension Manager (File > Extension Manager) ❶, then go to the "Default" tab ❷ and search for `lint` ❸. Click the "Disable" buttons in the results for one or both of them ❹. Close the Extension Manager and press F5 to reload Brackets.

![File > Extension Manager > Defaults > Search "lint" > Disable](https://i.ibb.co/Jt73v1W/disable-instructions.png)

## Use

When a JavaScript file is saved or opened, it will be checked by JSHint. In the bottom right corner there will be either a green checkmark, which means no problems were found:

![JSHint no problems](https://i.ibb.co/xDqCrQ8/no-problem.png)

Or there will be a yellow exclamation mark and a panel will come up at the bottom showing the problem(s):

![JSHint problems](https://i.ibb.co/vqwBx8d/problem.png)

If the panel is hidden, click the yellow exclamation mark to show it (or hide it again).

If you don't understand the problem, look at the [JSHint documentation](https://jshint.com/docs/) or search for JSHint's message.

## Configuration

Below are some ways to set JSHint configuration options with this extension. For these examples, will we be setting configuration for:

- ECMAScript 8 (`"esversion": 8`)
- jQuery (`"jquery": true`)
- In a browser environment (`"browser": true`)
- With developer debugging globals like `console` enabled (`"devel": true`)
- And three global variables: `myGlobal1`, `myGlobal2`, and `myGlobal3`

This is only an example, there are a huge number of possible settings. [There's a list of all configuration options supported by JSHint here](https://jshint.com/docs/options/). For more about JSHint configuration, see the [JSHint documentation](https://jshint.com/docs/).

### Settings for all projects

To modify settings for all projects in Brackets, open the brackets.json preferences file **(Debug > Open Preferences File)** and add or modify `"jshint.options"` and `"jshint.globals"`, like this:

![Example brackets.json change](https://i.ibb.co/RhZwtFX/options-example.png)

### Settings for a single project

Make a file in your project root called `.jshintrc` that contains the configuration settings, like this:

```json
{
	"esversion": 8,
	"browser": true,
	"devel": true,
	"jquery": true,
	"globals": {
		"myGlobal1": true,
		"myGlobal2": true,
		"myGlobal3": true
	}
}
```

[This](https://github.com/jshint/jshint/blob/main/examples/.jshintrc) is an example .jshintrc file with default values.

### Settings for a single file

You can add configuration directives at the start of your file that override other settings, like this:

```javascript
/* jshint esversion: 8, browser: true, devel: true, jquery: true */
/* globals myGlobal1, myGlobal2, myGlobal3 */

// My code...
```

Options for individual lines and much more in the [JSHint documentation](https://jshint.com/docs/).


## Updating JSHint

I'll try to keep this up-to-date, but it's easy to update JSHint manually (until some breaking change happens).

The copy of jshint.js used by the extension is identical to the official release, other than for a single line of code and a comment block to find it:

```javascript
    /**
     * PATCH for Brackets JSHint extension
     */
    module.exports = _;
```

Compare [the official release of v2.13.6](https://github.com/jshint/jshint/blob/0a5644f8f529e252e7dd0c0d54334ae435b13de0/dist/jshint.js#L18619) with [the extension's copy](https://github.com/tim-nz/brackets-jshint2/blob/ae3aa72ec6c1127f0feeefbab80d97a292125780/jshint/jshint.js#L18619) to see where it belongs.

Get the [new jshint.js](https://github.com/jshint/jshint/blob/main/dist/jshint.js) and copy those lines into the appropriate place. Go to the jshint folder in the extension folder, rename jshint.js (as a backup), then move the new version into its place. Press F5 to reload Brackets.

## Updates

[2023-08-02] **2.13.6**: Updated to JSHint 2.13.6. Using JSHint version as extension version from now on.



### Issues/Updates for original brackets-jshint:

> [2016-12-29] JSHint 2.9.4 thanks to hacker112
> 
> [2016-03-04] JSHint 2.9.1 thanks to hacker112
> 
> [2015-06-02] JSHint 2.8 back thanks to fix by marcelgerber
> 
> [2015-06-02] JSHint 2.8 pooped the bed. Bad to 2.6.3.
> 
> [2015-06-01] JSHint 2.8.0 - hopefully. On GitHub it was marked 2.8, but the comment on top still says 2.7, which I'm assuming is just a typo.
> 
> [2015-05-27] Adding support for global preferences
> 
> [2015-03-11] JSHint 2.6.3 @Hirse
> 
> [2015-02-07] JSHint 2.6.0
> 
> [2014-12-26] JSHint 2.5.11
> 
> [2014-12-10] temp version bump for reg
> 
> [2014-10-20] JSHint 2.5.6
> 
> [2014-08-22] JSHint 2.5.2
> 
> [2014-06-05] Two merges:
> 
> @D1SoveR - https://github.com/cfjedimaster/brackets-jshint/pull/53
> JSHint has added support for overrides within .jshintrc file, as per jshint/jshint#1401.
> Additional routine within the config loading procedure attempts to match this behaviour.
> 
> @busykai - https://github.com/cfjedimaster/brackets-jshint/pull/55
> Fix #52. Scan entire file tree for config.
> 
> Also, add jshint.scanProjectOnly extension pref. When set to true would only scan up to the project root. The default is false -- scan the entire tree up to the root.
> 
> In order to set the pref to limit the scan to project subtree, add the following to your brackets.json or to project's .brackets.json:
> 
> {
>     "jshint.scanProjectOnly": true 
> }
> 
> [2014-06-02] Merge in an improvement by hacker112 to add "extends" support to .jshintrc. Details here: https://github.com/cfjedimaster/brackets-jshint/pull/54
> 
> [2014-05-20] Fix by cgcgbcbc - update JSHint to 2.5.1
> 
> [2014-05-15] Fix by busykai - issue 49.
> 
> [2014-05-09] Yet another mod by busykai, this one is pretty big. It will now do 'proper' .jshintrc lookup - starting from current folder all the way up. Details at this PR: https://github.com/cfjedimaster/brackets-jshint/pull/48
> 
> [2014-04-14] Fix #46 - Basically just flesh out the package.json.
> 
> [2014-03-13] Fix #44
> 
> [2014-02-20] Includes two types fixes by mturkson23 and I added some text to the readme.md to explain how it works.
> 
> [2014-02-11] busykai fix for empty projects
> 
> [2014-02-09] Remove comments in .jshintrc fix by https://github.com/santhoshtr. Also updated to latest JSHint library.
> 
> [2014-01-08] lint and cleanup by busykai
> 
> [2013-12-10] License added by busykai
> 
> [2013-11-29] Pull req by busykai (updated readme, package.json)
> 
> [2013-11-28] Added display of the JSHint code. Fixes #21
> 
> [2013-11-19] Brought in a .jshintrc fix and new FS support by busykai
> 
> [2013-11-08] Updated to JSHint 2.3.0
> 
> [2013-11-02] User busykai (Arzhan "kai" Kinzhalin) restored the ability to handle .jshintrc
> files in the project directory. I was initially against this request as I thought it made
> sense to wait and see if the linting API would help make this simpler, but, crap, what's the
> point of source control/contributions/etc if we can't move nimbly, right?? Not sure if that
> even makes sense. Anyway, thanks to busykai the feature is back!
> 
> [2013-10-18] Oddly, JSHint isn't giving me a type anymore, *and*, in the array it returned 
> for a file, one entry was null. I don't get it - but i handle it.
> 
> [2013-10-08] Small mod to fix a loading issue with linting (temp)
> 
> [2013-10-08] Off by one error w/ line #.
> 
> [2013-10-07] Updated to the new Linting API. This required me to remove support for the default global config and per project
> config. The Linting API doesn't support an async response yet. In theory I could work around this, but for now I went
> the easier route. Don't forget that inline options should still work fine.
> 
> NOTE - There is a bug in Brackets where if you open up Brackets and JS file is automatically loaded, it will lint with
> JSLint. Just edit/save the file one time and it should switch to JSHint for good. See https://github.com/adobe/brackets/issues/5442.
> 
> [2013-10-05] jshint 2.1.11
> 
> [2013-08-15] So - I'm having some issues with the defaults. It appears as if you pass NO options to JSHint, it is a bit
> too lax. One option in particular, undef, must default to false, and that to me is a mistake. So I've added it to
> config.js. As I think I've said before - I'm looking for feedback on how to make this better.
> 
> [2013-08-12] jshint 2.1.9. Fixes https://github.com/cfjedimaster/brackets-jshint/issues/13.
> 
> [2013-07-11] Looks like a slight change to APIs made the language check break. This fixes https://github.com/cfjedimaster/brackets-jshint/issues/12
> 
> [2013-07-02] Another fix by DaBungalow - https://github.com/cfjedimaster/brackets-jshint/pull/11
> 
> [2013-06-16] Merged in a bootstrap fix by DaBungalow
> 
> [2013-06-04] Fixed a bug when .jshintrc didn't exist. Also switched to PanelManager. Note - if you do not
> see *any* JSHint results, I believe that is expected when no default options are specified. I think
> I may need to ship config.js with better default options.
> 
> [2013-05-24] Added package.json
> 
> [2013-05-21] Fixed a really dumb mistake. Thanks Jogchum Koerts.
> 
> [2013-05-20] Now supports reading .jshintrc files. Note - I do this on EVERY parse, and that is bad. I need
> to add some per-project caching. In my tests I couldn't tell that things had slowed down, but it really
> needs to be re-engineered to cache those checks.
> 
> [2013-05-20] I broke hiding jshint.
> 
> [2013-05-18] It should, hopefully, not act like JSLint in terms of autohiding when switch to a HTML file (and auto showing when you go to a .js file)
> 
> [2013-05-10] Updated to JSHint 2.0.0. The older library is still there in case you want to switch back.  
> 
> [2013-04-16] Tweak to menu add
> 
> [2012-11-12] Update code to properly insert the content over the status bar. Also made it resizable.  
> 
> [2012-09-26] Fix width issue. Thanks to Randy Edmunds for the reports.
> 
> Per feedback from Narciso Jaramillo, I use a checkbox to show enabled/disabled status and move to the item when you click a row.

Credits
=====
The extension is the work of [Raymond Camden](https://www.raymondcamden.com/) and the JSHint developers. All I have done is updated JSHint, expanded the instructions and changed the metadata. This is Raymond's credit from [the original repo](https://github.com/cfjedimaster/brackets-jshint):

> Built with [JSHint](https://jshint.com/) and heavily based on the work of [Jonathan Rowny](http://www.jonathanrowny.com/). 
