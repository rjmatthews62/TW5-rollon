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
Parameters may be surrounded by quotes if they are to contain spaces, e.g. table:"My Table" 

* table - the name of the table to roll on, or a dice expression like 3d6+1.  Required
* times - the number of times to roll, or a dice expression.  Defaults to 1.

### Tables ###
Tables are any Tiddler that contains:
* an unordered list (introduced with _*_
* an ordered list (introduced with _#_
* a definition list (introduced with __;__ on the term line and __:__ on the definition line.  Be careful that you alternate definitions and terms... you can tell if it's right by whether the terms are bold and the definitions below and indented)
* a sequence of lines beginning with numbers (and optionally having a __.__ after the number)

If the Tiddler has any prefatory material prior to the start of the first list in the Tiddler, that text is copied into the result.
If the Tiddler has any postscript material after the end of the last list in the Tiddler, that text is ignored.  This is useful for notes and annotations.

Lists don't actually have to be contiguous.  That is, since rollon is using a textual search, it will consider two lists in the same Tiddler as being the same as one.  That's probably a bug, but doesn't seem worth eliminating.  I wouldn't advise counting on that behavior to continue in future versions of Rollon Plugin.

### Behaviour ###
* If tiddler is tagged with **rollonResult**, then then result will be rendered on that page. To reroll, hit "edit" then "save" tiddler.
* If the table does NOT contain a recognizable list, it will be rendered as is. Macros will be expanded, so you can have render multiple rolls on a page.
* If tidder is **not** tagged with **rollonResult** it will render a button. When clicked, the button will create a new tiddler tagged with **rollonResult** and render that.
* Tables can have further `<<rollon>>` macros embedded which will be expanded. This can allow for quite complex tables. Take care not to create an infinite loop.


## RobbieMergewidget ##
This was written for a very specific purpose. The idea is to take a number of seperate rollon results and merge them.
Usage:

```
<$robbiemerge>
<<rollon Sometable>>
<<rollon AnotherTable>>
<$/robbiemerge>
```
This will parse the rollon macros, then merge the result into a single line. This exact usage probably won't be meaningful to anyone else in the world, but the techniques used to parse the child nodes into a string array may be useful.

## expander macro ##
The expander macro **expander_plugin.tid** will take text or a tiddler and recursively expand all macros ("<<" ">>" syntax only) but otherwise should return text unaltered.

This should simplify such tasks as using a template with macros to populate a new page.
### Parameters: ###
* **text** - text to expand.
* **tiddler** - tiddler to load and expand.

### Example ###
```
<$button actions='<$action-sendmessage $message="tm-new-tiddler" title="New Batcave Tiddler Ex" text=<<expander tiddler:Batcave>> tags="rollonResult"/>'>
New Roll on Batcave Ex
</$button>
```
