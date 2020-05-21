outline-todo.txt
================

**outline** is an extension for command line [todo.txt](https://github.com/ginatrapani/todo.txt-cli) that allows you to plan your projects in tab-indented outlines, and sync only the *[next actions](https://hamberg.no/gtd/#the-next-actions-list)* with todo.txt. 

A *next action* is defined as the first task with no subtask of its own in an outline with no dependent outlines.

*Next actions* inherit their parent task's priority, and any tags iwth a doubled sigil (e.g. ##tag, @@context, ++project).

Example
======

Our initial todo.txt contains the item "Walk dog"

    $ todo.sh ls
    1 (A) Finish tps reports @work
    2 Walk dog

However, we own neither a dog nor a leash, so we will create use **outline** to organize this project:

    $ todo.sh outline mkol WalkDog 2

The file *WalkDog.ol.txt* is created, the task item moved to the new file, and we edit the outline as follows:

    Walk dog
    	buy leash @shopping
    	adopt dog

After import, *todo.txt* contains:

    $ todo.sh ls
    1 (A) Finish tps reports @work
    2 buy leash @shopping outline:WalkDog

And *WalkDog.ol.txt* contains:

    Walk dog
    	adopt dog

Let us suppose that we find that buying the leash should be considered a project in it's own right:

    $ todo.sh outline mkol BuyLeash 2
    Choose parent project outline:
    	1) WalkDog.ol.txt
	2) NONE
	#? 1

The above creates the outline *WalkDog.BuyLeash.ol.txt*. Since, this outline is a dependency of *WalkDog.ol.txt*, 'adopt dog' will not be imported until *WalkDog.BuyLeash.ol.txt* has been completed.


Installation
=====

Add *outline* and *ol* to your todo.txt addon directory. In order to automatically import next actions automatically after each time *todo.sh archive* runs, also add *archive* to the addon directory.

Usage
======

outline|ol addto|append PROJECT "TEXT..."
	Append TEXT to outline with name containing PROJECT.
	
	If more than one outline name contains PROJECT, select outline
	from menu.
	
outline|ol edit PROJECT
	Open outline with name containing PROJECT using program specified
	in \$EDITOR.
	
	If more than one outline name contains PROJECT, select outline
	from menu.

outline|ol help
	Display this page
	
outline|ol import
	Next actions, as defined below, are imported into todo.txt from outline
	files in todo directory, unless blocked by an existing subtask.
	
outline|ol mkol PROJECT [#ITEM]
	Create an outline file corresponding to PROJECT, and move task at line
	#ITEM in todo.txt to the outline. A menu to select a parent outline from
	the existing outlines is presented.	

outline|ol mv #ITEM PROJECT
	Move todo task at #ITEM in todo.txt to outline with name containing 
	PROJECT.

	If more than one outline name contains PROJECT, select outline
	from menu.

outline|ol prepend PROJECT "TEXT..."
	Add TEXT as a line to the begging of outline with name containing PROJECT.

	If more than one outline name contains PROJECT, select outline
	from menu.

outline|ol import
	Imports next action(s) from outlines into todo.txt. For each outline,
	the existence of a child outline in the todo directory, or of a task
	in todo.txt labeled as belonging to this outline or a child outline
	will block import.

	Outline files:

	Files in the todo directory ending in ".ol.txt" are regarded 
	as outline files.  Each file should be a tab-indented outline without 
	blank lines.

	The outline filename format is:
	"grandparentproject.parentproject.project.ol.txt"

	Parent projects in the filename indicate task dependency, and tasks from
	"parent.ol.txt" will be blocked from import as long as
	"parent.child.ol.txt" is present in the todo directory.

	Next Actions:

	A next action is identified as an active task with no subtask, where 
	an active task is the first item in the outline or the first subtask
	of another active task. 

	An item's status as an active task or next action can be modified by
	various tags described below.

	Outline tags:

	#next	Marks an outline item as active.
	#list	Marks every direct subtask of an item as active.
	#block   Blocks the item from being identified as a next action.
	#waitfor:PROJECT   Blocks item as next action until 
	"+PROJECT" is absent from todo.txt and there is no outline file in the 
	todo directory corresponding to PROJECT.

Tag and priority inheritancej:

	Tags are words that have been prepended with the sigil #, @, or +. 
	Tags with doubled sigils, as in @@context, are inherited by subtasks. 
	This includes the special outline tags described above, and their 
	effects. Tasks with priority indicated by, e.g. "(A)" have their 
	prioirty inherited by any subtasks. 

Configuration
=============

Towards the top of `outline`, you will find the following configuration options:

`OL_AUTO_IMPORT`: Automatically import after every action that changes an outline. Enabled by default.

`EDITOR`: Text editor to launch for editing outline. Vim by default.

`OL_FILENAME_ENDING`: Ending of outline file names. `.ol.txt` by default.

`OL_FILENAME_DELIMITER`: Delimiter of project names in filename, `.` by default.

`OL_INDENT_CHARS`: String that marks how indentations in the outline. TAB character by default.

`OL_NEXT_TAG`: `#next` by default. Identifies this item's task tree as active. Use `##next` to activate all subtrees.

`OL_LIST_TAG`: `#list` by defualt. Direct child task trees are active when task is active. Useful for shopping lists, etc 

`OL_BLOCK_TAG`: `#block` by default. Prevents item from being identified as next action.

`OL_WAITFOR_LABEL`: `#waitfor` by default. `#waitfor:PROJECT` will block identification as next action until there is no corresponding active project in todo.txt or outline file.
