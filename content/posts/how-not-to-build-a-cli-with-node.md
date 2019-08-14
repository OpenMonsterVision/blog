---
title: "How Not to Build a Cli With Node"
date: 2019-08-13T17:21:28-05:00
tags : [ "unix","dependencies","linux","bash","cli","node"]
draft: true
---

# The Who
I'm just a dude, that writes shit down. Occasionally I write something useful for sys admins, but mostly my [github](https://github.com/bsdpunk) is a garbage place where ideas go to die. I am [@bsdpunk](http://twitter.com/bsdpunk), and I am a terrible person. And mostly I write shells that don't currently work, but maybe did at one time, or maybe can if you give me a minute.

# The What

[This guy](https://twitter.com/dkundel?lang=en) wrote a [post](https://www.twilio.com/blog/how-to-build-a-cli-with-node-js) I don't agree with and I'm going to complain about it. Did this guy actually do anything wrong? No, of course not. I just like shouting at the ether. 

# The Why
Cause someone put cement in my wheaties. Who cares, you're here now?


# What's wrong with this cli application

## The Design feels incomplete

Why can't I type:

``` bash
create-project typescript --git --yes dirName
```
Or in my case, since I don't like dashes(or any operators), in my command line:
``` bash
createp typescript --git --yes dirName
```
Why am I making the directory, the rails app, isn't

``` bash
mkdir newApp
cd newApp
rails new
```

It's:

```
rails new newApp
```
To create a new hugo blog, I don't type:
```
mkdir blog
cd blog
hugo new site blog
```
I just type:
```
hugo new site blog 
```

## It violates the Unix Way

Yeah, this is an old man gripe, and I'm not old enough to be griping about it. Don't care, doing it anyway. Unix command line tools should be good at one thing, and they should always be able to take orders from stdin. This program does not allow you to take stdin as your arguements. If I want I should be able to:
```
echo "typescript --git dirName" | createp
```

This by their nature makes them scriptable.

Also, command line tools make assumptions, and they rarely send you into an interface. There's reason for this. Nobody wants to type a bunch of unnessecary flags, and even more than that, no one wants to have a script they're writing hang, because it went into an interactive mode. What, now I got start writing expect scripts for everything, no way. If you want to develop an interactive tool, then you need to make either a shell, complete with tab completion, and every other bell and whistle anyone could want, or you have an explicit flag for interactive mode.

And if you think I shouldn't harp on the interactive part, as that is the I in CLI, well I got something for you there too. Actual CLI's shouldn't be a choice or two...this should be shells. They should be true interactive experiences, check out this actual shell I wrote, [kish](https://github.com/bsdpunk/kish).

## The same thing that's wrong with every Node project
To many goddamn dependencies, an npm ls of the project:

``` bash
$ ls node_modules/
@samverschueren		code-point-at		figures			listr			npm-run-path		run-async		supports-color
@types			color-convert		get-stream		listr-silent-renderer	number-is-nan		rxjs			symbol-observable
ansi-escapes		color-name		has-ansi		listr-update-renderer	object-assign		safer-buffer		through
ansi-regex		cross-spawn		has-flag		listr-verbose-renderer	once			semver			tmp
ansi-styles		date-fns		iconv-lite		lodash			onetime			shebang-command		tslib
any-observable		elegant-spinner		indent-string		log-symbols		os-tmpdir		shebang-regex		type-fest
arg			emoji-regex		inquirer		log-update		p-finally		signal-exit		which
chalk			end-of-stream		is-fullwidth-code-point	merge-stream		p-map			slice-ansi		wrap-ansi
chardet			escape-string-regexp	is-observable		mimic-fn		path-key		string-width		wrappy
cli-cursor		esm			is-promise		mute-stream		pkg-install		strip-ansi
cli-truncate		execa			is-stream		ncp			pump			strip-eof
cli-width		external-editor		isexe			nice-try		restore-cursor		strip-final-newline

```
That's 81...you know....not counting there dependencies.
``` bash
$ ls node_modules/ | wc -l
      81
```


# Guided Tour

## Install this Twilio Shit

It's twillio's blog of course they want you to install all the twilio garbage. Now that you got that working, lets move on and not refrence twilio shit again.

## Setup your first CLI !@#!@!@#!#!@#!@#!@

Now, Watch a goddamn video. I didn't watch that shit, it was like 15 minutes long. who has time like that? 

Bunch of command line stuff, pretty normal. 


Don't run this:
``` bash
# Don't run this, clearly I'm being a jackass.
mkdir createp; cd createp; mkdir src; echo "export function cli(args) { console.log(args);}" > src/cli.js; echo -e "#!/usr/bin/env \n noderequire = require('esm')(module /*, options*/);require('../src/cli').cli(process.argv);" > bin/creatp;
```
You know normal set up stuff. Get to your first package install. Not a bid deal one package. Obviously I took the dash out, don't know why javascript guys constantly want to put operators in names. Then he's like hey, install these two dependencies but those guys have dependencies and so on, now we have 36 packages.

Lets make some more code edits, ok now install a couple of more packages, these don't seem that bad(ncp, chalk) I think they only install one more additional package.

By the way I had to change the template directory doo hickey on mine cause like, fucked dir. I put a quoted abs path as a quick fix.

Lines 24-27:

``` javascript
new URL(currentFileUrl).pathname,
   "/Volumes/Seagate Backup /work/git2/createp/templates",
   options.template.toLowerCase()
 );
```
Anyway more code and npm installs. We're finally done and it looks like 81ish node_modules for this quite broken CLI.


# OK, so you've been mean but your not improving on anything?
Well of course I did, I made a node tool, with a single dependency even recycling brohaims code. Cause despite all my complaints it's clear to me that guy knows how to code better than I do, we just don't agree on other stuff.

