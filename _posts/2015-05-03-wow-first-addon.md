---
layout: post
title: World of Warcraft first addon
---

I have tried to write my own simple addon for WoW. There are many tutorials about creating addons. But, many of them are deprecated or not give you the full understanding about addons structure. I would like to write my own experience.

It will be a very simple addon, even without buttons. This addon will show coordinates, azimyth and pith in a small window. Addon you can find [here](https://github.com/freekode/TestAddon/).

Also there are some libs for developing. And next tutorial will be about Ace3 library.

## Addon structure
As you know addons store in "World of Warcraft/Interface/AddOns/".
Addon folder contains TOC file, LUA and XML files.
{% highlight text %}
TestAddon/
    TestAddon.toc
    <.lua>
    <.xml>
{% endhighlight %}

TOC - (Table Of Contents) required file, must be the same as directory name, provide information about your addon to the game (files, SavedVariables, Version, Tittle, etc.)<br>
LUA - the code<br>
XML - addon interface

Of course, you can create directories in the main addon folder. Actually I doing it, I have lua/ and xml/ folders. But main xml I store in root addon directory.

## TOC
First, create a ColorEncode.toc file, with this content:
{% highlight text %}
## Interface: 60100
## Title: Test Addon
## Notes: Addon for testing
main.xml
{% endhighlight %}

`## Interface: 60100` - version of the game where your addon should work, if it less than last version, in the game you will see warning about out of date addon.<br>
`## Title: Test Addon` - title of your addon, can be more readable<br>
`## Notes: Show your coordinates, azimyth and pitch` - description<br>
`main.xml` - files, actually you can also write here your lua scripts, but is not necessary, because your xml file will include they.

This parameters enough, [here](http://www.wowwiki.com/TOC_format) you can find more.

## XML
We create one simple xml file which describe an interface of our addon.

Begin with the base of xml file:
{% highlight xml %}
<Ui xmlns="http://www.blizzard.com/wow/ui/"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xsi:schemaLocation="http://www.blizzard.com/wow/ui/
    ..\..\FrameXML\UI.xsd">

  <Frame name="MainFrame"
         parent="UIParent">
  </Frame>
</Ui>
{% endhighlight %}



More about `<Frame>` you can find [here](http://www.wowwiki.com/XML/Frame).

---

## Useful links/books
<http://wow-pro.com/general_guides/jahwo039s_addon_writing_guide_0><br>
<http://wowprogramming.com/><br>
<http://www.wowwiki.com/HOWTOs><br>
<http://habrahabr.ru/post/113258/><br>
<http://www.wowace.com/addons/ace3/><br>
<http://wow.gamepedia.com/WelcomeHome_-_Your_first_Ace3_Addon>

[Daniel Gilbert, James II Whitehead - Hacking World of Warcraft](https://www.goodreads.com/book/show/279492.Hacking_World_of_Warcraft)<br>
[James Whitehead II, Rick Roe - World of Warcraft Programming: A Guide and Reference for Creating WoW Addons](https://www.goodreads.com/book/show/6952656-world-of-warcraft-programming)<br>
[Paul Emmerich - Beginning Lua with World of Warcraft Add-ons](https://www.goodreads.com/book/show/6395549-beginning-lua-with-world-of-warcraft-add-ons)
