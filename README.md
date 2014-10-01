outline-todo.txt
================

**outline** is an extension for command line [todo.text](https://github.com/ginatrapani/todo.txt-cli) that allows you to plan your projects in a tab-indented outline, and sync only the *next actions* with todo.txt. Actions for editing and displaying your outline, syncing outline.txt with todo.txt, and completing tasks directly from the outline, are also provided.

A *next action* is defined as the first subtask with no subtask of its own under 1) a root task or 2) a task tagged with a project (eg. "+project")--unless the task or parent tree is tagged with *@someday* or *@maybe* (or as otherwise configured).

*Next actions* inherit the project tags and the nearest priority tag from their parent tree. Context tags ("@context") are not inherited unless written with two "@" symbols ("@@heritablecontext"). 

Example
======

The following example illustrates how **outline** identifies *next actions* and synchronizes *todo.txt* and *outline.txt*.

Our initial *todo.txt*

    Finish TPS report @work
    Mow lawn

Our initial *outline.txt*

    (B) Walk the dog +dogwalk
    	Get a dog
		    Lookup animal shelters 
    	Buy a +leash
		    Lookup pet stores
    	Lookup dog parks
    Call mom @phone
    @someday
	    get my GED

Sync command

    todo.sh outline sync
    
Our updated *todo.txt*

    (B) Lookup animal shelters  id:1 +dogwalk
    (B) Lookup pet stores id:2 +dogwalk +leash
    Call mom @phone id:3
    Finish TPS report @work id:4
    Mow lawn id:5


Our updated *outline.txt*

    (B) Walk the dog +dogwalk
    	Get a dog
    		Lookup animal shelters id:1
    	Buy a +leash
	    	Lookup pet stores id:2
    	Lookup dog parks
    Call mom @phone id:3
    @someday
    	get my GED
    Finish TPS report @work id:4
    Mow lawn id:5

The id tags are to allow syncrhonization between todo.txt, outline.txt, and done.txt if an item has been edited.

Installation
=====

Add *outline* and *ol* to your todo.txt addon directory. 

Configure outline filename, location, and tags to block identification as *next action*, if necessary by editing the block underneath 
    
    # == CONFIG ==

in *outline*

Usage
======

    Usage: todo.sh outline action [options]
        todo.sh ol action [options]

    Note: tab indented outline, outline.txt, should be present
      in todo.txt directory

    Actions:
    add LINE_NUMBER INDENTATION "Task text"
    a LINE_NUMBER INDENTATION "Task text"
        Adds task text to line LINE_NUMBER with INDENTATION tabs

    addafter LINE_NUMBER "Task text"
    aa LINE_NUMBER "Task text"
        Adds line of text in outline.txt below
        line number LINE_NUMBER, with same indentation

    addbefore LINE_NUMBER "Task text"
    ab LINE_NUMBER "Task text"
        Adds line of text in outline.txt before
        line number LINE_NUMBER, with same indentation

    addsubtask LINE_NUMBER "Task text"
    as LINE_NUMBER "Task text"
        Adds line of text in outline.txt in line below
        with additional indentation

    export
    e
        Removes next actions from outline.txt which have been completed
        Labels any new next actions in outline.txt with id:<number>.
        Copies next actions from outline.txt to todo.txt unless already present
        Removes items from todo.txt that are no longer next actions in outline.txt

    import
    i
        Copies new items from todo.txt to outline.txt

    list [LINE_NUMBER]
    ls [LINE_NUMBER]
        If LINE_NUMBER is specified, displays immediate subtasks
        of LINE_NUMBER. Otherwise displays top level tasks.

    listall [LINE_NUMBER]
    lsa [LINE_NUMBER]
        Displays all subtasks of LINE_NUMBER. If LINE_NUMBER
        not specified, displays entire outline.

    move SRC_LINE DEST_LINE [NUM_TABS]
    mv SRC_LINE DEST_LINE [NUM_TABS]
        Moves source line to destination line in outline.txt
        If indentation is not specified, original indentation is preserved

    next
    n
        Displays next actions in outline.txt
        Each top level task and item tagged with a project
        (i.e. "+project") has one next action: the task with
        with no subtask beneath it or sibling task above it

    rm LINE_NUMBER
    del LINE_NUMBER
         Removes specified line from outlook.txt

    sync
    s
        Removes next actions from outline.txt which have been completed
        Copies new items from todo.txt to outline.txt
        Labels any new next actions in outline.txt with id:<number>.
        Copies next actions from outline.txt to todo.txt unless already present
        Removes items from todo.txt that are no longer next actions in outline.txt

    tab LINE_NUMBER NUM_TABS
    t LINE_NUMBER NUM_TABS
        Adjusts indentation of specified line to specified tabs
