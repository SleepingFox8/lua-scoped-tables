# lua-scoped-tables

This repository specifies how to store variables in tables to eliminate the risk of creating accidental global variables in LUA.

## Tables

These tables are given a specific scope, and are used to hold all variables.

### GLBL

Stores all Global variables. Must be initialized at the begining of LUA script if it has not been already.

```lua
--initialize GLBL table if needed
    if GLBL == nil then
        GLBL = {}
    end
```

### SCRIPT

Stores variables that are globally accessible by it's .lua file, but not by other files or threads. Must be initialized at the begining of LUA script, outside any scoped structure such as functions or if statements.

```lua
--initialize SCRIPT table
--Stores global variables for just this script
    local SCRIPT = {}
```

### FUNC

Stores variables inside the scope of a single function. Function arguments should also be moved into this table before doing anything else with them. While function arguments are already naturally local to that function, without the FUNC table in front of a variable, the scope of the variable is is not immediatly obvious to the programmer.

```lua
function myFunction(arg)
    --initialize function
        --initialize function table
            local FUNC = {}
        --store arguments in known scoped table
            FUNC.arg = arg
    -- function body goes here
        -- ...
end
```

### MAIN

The same as the ``FUNC`` table but used for the MAIN program of a script, which is outside the scope of any function.

## Variables

All variables should be stored as properties inside a table of a known scope. Any variable outside of a table is to be considered malpractice and dangerous. This makes it obvious what scope a variable is in, because the name of the variable's scope should be the name of the table that variable is inside of.

Good variable assignments:
```lua
GLBL.var1 = "bob"
SCRIPT.var2 = "alice"
MAIN.var3 = "Hello World!"
```

Bad variable assignments:
```lua
myGlobalVariable = "test"
local myLocalVariable = 123
```
