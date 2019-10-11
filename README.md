# TW5-rollon
Convert original classic Rollon plugin to TW5 macro.

Original documentation here: http://rollon.tiddlyspot.com/ by Joshua Macy

This required a significant restructuring. At present the only "table" and "times" are implemented, but that covers quite a lot of the functionality.

## To Install
Import newroller.tid into your TW5 Tiddlywiki. Reload.

## Some useful notes
I found the TW5 documentation on plugins, widgets and macros hard to navigate, but fairly straightforward to implement when I did eventually find the useful links.
* Explains what a javascript macro is: https://tiddlywiki.com/dev/index.html#JavaScript%20Macros
* Classic TW every code addon on was referred to as a plugin. In TW5, a plugin is merely a way of packaging up multiple files into a easily installable group. I wasted a lot of time looking for documentation on plugins, when all you probably need is to know how to write a macro.
* A macro simply takes arguments and returns a string which is then rendered.
* For more complex tasks, you may want a widget. This was very a very helpful tutorial: https://btheado.github.io/tw-widget-tutorial/
* The partcular structure of most TW js code is a *Self-Invoking Function* ... **(function(){ some code})();** ... this is the term to search for if it puzzles you. (I was puzzled originally.)
* Most wiki funcitions are accessible through either **$tw.wiki** or **this.wiki** while running JS code.

## Documentation ##
Usage **<<rollon tablename|table:string|dice_expression [times|times:number or dice_expression] >>**

### Parameters ###
Parameters may be surrounded by quotes if they are to contain spaces, e.g. table:"My Table"  Any parameter except table may be passed *table - the name of the table to roll on, or a dice expression like 3d6+1.  Required
*times - the number of times to roll, or a dice expression.  Defaults to 1.  Prompt will prompt for a number or dice expression when you click

### Tables ###
Tables are any Tiddler that contains:
* an unordered list (introduced with _*_
* an ordered list (introduced with _#_
* a definition list (introduced with _;_ on the term line and _:_ on the definition line.  Be careful that you alternate definitions and terms... you can tell if it's right by whether the terms are bold and the definitions below and indented)
* a sequence of lines beginning with numbers (and optionally having a _._ after the number)

If the Tiddler has any prefatory material prior to the start of the first list in the Tiddler, that text is copied into the result.
If the Tiddler has any postscript material after the end of the last list in the Tiddler, that text is ignored.  This is useful for notes and annotations.

Lists don't actually have to be contiguous.  That is, since rollon is using a textual search, it will consider two lists in the same Tiddler as being the same as one.  That's probably a bug, but doesn't seem worth eliminating.  I wouldn't advise counting on that behavior to continue in future versions of Rollon Plugin.

