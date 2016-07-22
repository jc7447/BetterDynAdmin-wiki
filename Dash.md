#DASH - DynAdmin SHell

Table of Contents
=================

  * [DASH - DynAdmin SHell](#dash---dynadmin-shell)
    * [Start Dash](#start-dash)
    * [Navigation](#navigation)
      * [Console](#console)
      * [Editor](#editor)
    * [Component Reference:](#component-reference)
     * [@This](#this)
    * [Saving a result to a variable](#saving-a-result-to-a-variable)
    * [Result filtering](#result-filtering)
      * [Single value:](#single-value)
      * [Multiple Values](#multiple-values)
    * [Functions](#functions)

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

### @this

@this is a keyword that references the current component. 

Ex:

> $> set @this.loggingDebug true

Avoid using it in scripts because the calling location might change

## Saving a result to a variable

You can save the output of a command to a variable, using ">varName". You can then later reference the value using "$varName"

Ex:

> $> echo foo >bar  
> foo

> $> echo $bar  
> foo

*Note: For now the initial result is always prompted, even if you save to a variable - it's not a standard output like in a real shell.*

## Result filtering

You can save part of a result to a variable. There are two ways : either save a single value, or a set:

### Single value:

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

### Multiple Values

> \>varname(key1.path1,key2.path2)

You can write that over several lines but you must keep the first "(" on the same line as the command

> $> print @OR sku someId >varname(  
> skuId:id,  
> parent:parentProduct  
> )

> $> echo $varname  
> {skuId:"somevalue",parent:"parentValue"}

## Functions

### help

* displays the list of all the functions and some quick tips

### history

> $> history [clear]

* Shows the command history
* options:
 * Clear: clears the history. This will also clear the persisted history and the console typeahead data source.

### go

> $>go /to/some/Component  
> $>go @SHORT  
> $>go $var
 
 * Redirects the user to the component page

### get
> $>get /some/Component.property

* Returns the value of Component.property

### set
> $>set /someComponent.property newvalue

* Sets the value of Component.property to newvalue. Returns the property value after setting it to validate.

### call
> $>call /some/Component.method

*Call the *method* method on component *Component*

### print

> $>print  /some/Repository itemDescriptor id

* Runs a print-item xml query and returns the result

### rql

> $>rql /some/Repo {xmlStatement} [ [comma,separated,params] ]
> $>rql /some/Repo.savedQuery [ [comma,separated,params]  ]

*Note: the parameters are optionnal, and between []*

* Runs an xml query on the repository and returns the result
* The value between the { } can be split on several lines, but the { must be on the first and the } on the last
* The form *Repo.saveQuery* allows to call a previously saved query. Use the [queries](#queries) function to display the queries of a repository.
* The params will replace, in order, any token of form $N where N is a number.

### queries

> $>queries [/some/Repository]

* Prints all the saved queries : name & content
* If a repository is precised, only prints the queries of this repository