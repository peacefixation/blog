---
title: "Asterisk syntax highlighting extension for Brackets"
date: 2016-03-31 12:46:37 +1100
draft: false
toc: false
images:
categories:
  - development
tags: 
  - development
  - asterisk
  - brackets
---

I recently started using the [Brackets](http://brackets.io/) text editor. Brackets is a powerful open source text editor in much the same vein as Sublime with lots of nice programming features and support for extensions. Some of the files that I edit are Asterisk configuration files. Asterisk is a VoIP server and these are the files that tell it how to route VoIP calls. They have their own special syntax and Brackets didn't include a syntax highlighter by default, so I thought I might make one.

As it turns out it's not so hard and there's a [How To Write Extensions](https://github.com/adobe/brackets/wiki/How-to-Write-Extensions) guide on the Brackets Github repo. Of particular interest is the [Language Support](https://github.com/adobe/brackets/wiki/Language-Support) section. Here I discovered that Brackets actually leverages the Code Mirror syntax highlighters, and since there's already an Asterisk language mode there, I could just reference that and be on my way. Well, almost. So here's how I made the extension.

Open the Brackets extension folder, `Help > Show Extensions Folder`

Open the `users` folder and create a new folder for your extension.

Create a `main.js` file inside your extension folder.

To add language support for a language with an existing [Code Mirror mode](http://codemirror.net/mode/), add the following to your main.js where the value for `mode` matches the Code Mirror mode. This part took me the longest because it wasn't actually clear what the Code Mirror mode for Asterisk was called, given that in the list it's referred to as "Asterisk dialplan". After some trial and error I discovered that the name is the directory name in the URL `http://codemirror.net/mode/asterisk/index.html`, i.e. "asterisk".

If your language does not have an existing Code Mirror mode, you'll need to write it yourself. That's beyond the scope of this article, so I'll leave it as an exercise for you.

This code is based on the example in the [Language Support](https://github.com/adobe/brackets/wiki/Language-Support) section. Note that I'm defining a name that will appear in the list of languages, the Code Mirror language mode, a file extension to associate this language with (I ended up removing it because `.conf` files are far too common to associate with Asterisk), and a single line comment character.

{{< highlight js >}}
define(function (require, exports, module) {
    var LanguageManager = brackets.getModule("language/LanguageManager");

    LanguageManager.defineLanguage("asterisk", {
        name: "Asterisk",
        mode: "asterisk",
        //fileExtensions: ["conf"],
        lineComment: [";"]
    });
});
{{< / highlight >}}

While you're hacking away you can reload changes to the extension from the Brackets menu `Debug > Reload With Extensions`. There's a lot more information on debugging in the Brackets extension guide, so I won't replicate that here.

When you're ready to publish your opus, you need to create a `package.json` file. Refer to the Brackets extension guide for an [example](https://github.com/adobe/brackets/wiki/Extension-package-format#packagejson-format).

Then package your extension files in a `.zip` archive and upload it to the [Brackets Extension Registry](https://brackets-registry.aboutweb.com/).

Your extension will now be available from the Brackets menu `File > Extension Manager`, and viola, you're done!

You can see the code for this little extension on [Github](https://github.com/peacefixation/AsteriskSyntaxHighlighting).

