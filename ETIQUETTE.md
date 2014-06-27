##Patch Etiquette

By applying patches to the Lua codebase you are essentially creating
a new, Lua like, language. According to the [source license](http://www.lua.org/license.html) used by Lua you have unrestricted access to the Lua source code, to do with as you see fit. However, you do not have permission to use the Lua name, as PUC-Rio has the copyright.

Also, releasing your new Lua-like langage under the name "Lua" would just cause confusion for anyone trying to use both your patched version and the original version of Lua at the same time.

I have outlined the steps to name your new language below:

###lua.h

    /*
    ** $Id: lua.h,v 1.302 2014/03/20 19:42:35 roberto Exp $
    ** Lua - A Scripting Language
    ** Lua.org, PUC-Rio, Brazil (http://www.lua.org)
    ** See Copyright Notice at the end of this file
    */
    
    #ifndef lua_h
    #define lua_h
    
    #include <stdarg.h>
    #include <stddef.h>
    
    #include "luaconf.h"
    
    #define LUA_VERSION_MAJOR	"5"
    #define LUA_VERSION_MINOR	"3"
    #define LUA_VERSION_NUM		503
    #define LUA_VERSION_RELEASE	"0 (work2)"
    
    #define LUA_VERSION	"Lua " LUA_VERSION_MAJOR "." LUA_VERSION_MINOR
    #define LUA_RELEASE	LUA_VERSION "." LUA_VERSION_RELEASE
    #define LUA_COPYRIGHT	LUA_RELEASE "  Copyright (C) 1994-2014 Lua.org, PUC-Rio"
    #define LUA_AUTHORS	"R. Ierusalimschy, L. H. de Figueiredo, W. Celes"
    
    
    /* mark for precompiled code ('<esc>Lua') */
    #define LUA_SIGNATURE	"\033Lua"
    
    /* option for multiple returns in 'lua_pcall' and 'lua_call' */
	#define LUA_MULTRET	(-1)
    
	{snip}


The values of `LUA_VERSION_MAJOR` and `LUA_VERSION_MINOR` need to be set, if this is your first version then `1` and `0` are good choices.

Once those two values are set you will use them to set `LUA_VERSION_NUM`. The formula is simple: `LUA_VERSION_MAJOR` x `100` + `LUA_VERSION_MINOR`. So if you used `1` and `0` before the value for `LUA_VERSION_NUM` would be `100`. Please note that this field does NOT have quotes!

The `LUA_VERSION_RELEASE` entry is the third value in the version number, typcially used for bug-fix releases. You may also specify a special status, such as `work 2` in this field. A good default for this field is `0`.

Now that you have set the version number of your new language you need to set the name. To edit the textual version information you need to change a few fields:

To change the name you need to edit `LUA_VERSION` and edit the `"Lua "` part. Note there is a space between the last letter and the closing quote mark.

You will then want to change the `LUA_COPYRIGHT` and `LUA_AUTHORS` fields. This is okay as once you started hacking Lua you created a new language, you are the author and the copyright belongs to you. As per the licence agreement, however, you are required to reproduce the original PUC-Rio copyright notice in all copies or substantial portions of the original software.

This notice can go in your documentation, or you can reproduce it somewhere in your program. Do not use the Lua logo or name in association with your new scripting language. Somewhere in your about documentation you can give credit to the Lua authors and/or say your language is a variant of Lua.

The changes so far will change the Lua version string information for your new Lua variant. However, two more final changes are required to change the name of your executable and compiler binaries.

###lua.c
    /*
    ** $Id: lua.c,v 1.210 2014/02/26 15:27:56 roberto Exp $
    ** Lua stand-alone interpreter
    ** See Copyright Notice in lua.h
    */
    
    
    #include <signal.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <string.h>
    
    #define lua_c
    
    #include "lua.h"
    
    #include "lauxlib.h"
    #include "lualib.h"
    
    
    #if !defined(LUA_PROMPT)
    #define LUA_PROMPT		"> "
    #define LUA_PROMPT2		">> "
    #endif
    
    #if !defined(LUA_PROGNAME)
    #define LUA_PROGNAME		"lua"
    #endif
    
    #if !defined(LUA_MAXINPUT)
    #define LUA_MAXINPUT		512
    #endif



###luac.c


