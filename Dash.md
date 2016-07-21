#DASH - DynAdmin SHell

WIP!!

## Start Dash

* Open Dash by either:
 * Clicking on the Dash $ button in the menu
 * Using the "ctrl+alt+T" shortcut

## Navigation

The modal is separated in 2 sections : the screen (A) and the input section.

The input section has 2 modes : console & editor.

### Console

![Console Tab](https://raw.githubusercontent.com/jc7447/BetterDynAdmin/dev/resources/dash/dash.main.png)

* A : The commands results are displayed here.

1. Clears the screen
1. Closes the modal
1. Repeat command
1. Clear the line
1. Command prompt - hit **enter** to submit.  
    The command prompt has typeahead : it is filled with functions (with their params pattern) and commands from the history.
1. Clears the input
1. Switch to editor mode

### Editor

![Console Tab](https://raw.githubusercontent.com/jc7447/BetterDynAdmin/dev/resources/dash/dash.editor.png)

* A : Editor input zone

1. Saved script selector
1. Opens the selected script in the editor
1. Deletes the selected script
1. Clears the editor and name section (A, *7)
1. Runs the content of the editor
1. Saves the content of the editor using name set in *7
1. Input for script name


## Component Reference:

You can refer to a favorited component by using its short name, prefixed with **@**  

Ex: @OR for /atg/commerce/order/OrderRepository  

You can test the value with an "echo" command:

> $> echo @OR  
> /atg/commerce/order/OrderRepository 

If a shortname refers to several component, ex: OR: OrderRepository and OtherRepository, you can select one of them by specifiying an index : @OR#0 or @OR#1.  

To know the index, type the command *comprefs*. If you don't mention the index, you will be prompted with an error and the possible values

## Saving a result to a variable

You can save the output of a command to a variable, using ">varName". You can then later reference the value using "$varName"

Ex:

> $> echo foo >bar  
> foo

> $> echo $bar  
> foo

*Note: For now the initial result is always prompted, even if you save to a variable - it's not a standard output like in a real shell.*

### Result filtering

You can save part of a result to a variable. There are two ways : either save a single value, or a set:

### 1: Single value:

> \>varName:some.path.in.the.output

Ex:  
> $> someFunctionThatReturnsAnItem >skuId:id  
> $> echo $skuId  
> theSkuId

Assuming a complex structure is returned by the function

{some:{path:{id:[a,b,c]}}}

> $> echo $someComplexObject >value:some.path.id.2
> $> echo $value
> c

### 2: Multiple Values

> \>varname(key1.path1,key2.path2)

You can write that over several lines but you must keep the first "(" on the same line as the command

> $> print @OR sku someId >varname(  
> skuId:id,  
> parent:parentProduct  
> )

> $> echo $varname  
> {skuId:"somevalue",parent:"parentValue"}

## Functions

* help : displays the list of all the functions and some quick tips
* go : 
 * usage:  go /to/some/Component : redirects the user to this component page