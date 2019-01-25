# ASCII-Mapper-2.0

New, more powerful and using a better programming style!

I do not recommend to use the previous version of my ASCII-Mapper since the code is really bad! I ashamed of it, because I use a bunch of global variables and the structure is bad to. But let us talk about nicer things like the version 2.0 !

## What can I do with ASCII-Mapper 2.0?

If you go to a new area, you can active ASCII-Mapper 2.0 and it will save your movements. At call, it will draw you a map of the region, restricted to the already visited rooms, in ascii art like this:


 	 Z           
 	  \          
 	   \   |     
 	   -O--O     
 	    |\/|     
 	    |/\|     
 	   -O--O--O-



If you prefer a smaller version, you can also get


	Z       
 	 \  |   
 	 -O-O   
 	  |X|   
 	 -O-O-O-


 WARNING: This will just work in regions with the following conditions:
- Each room must have a unique room id
- The rooms should be structured on a grid.
- The exists has to be limited to the eight main directions, up and down.

The second condition is very important. If you go north and back south, you should arrive at the same room as you started.
And if you go northeast, you go one unit north and one unit east at the same time.
Hence, if you go northeast and then southeast, you went two units to east and you have to go two units
to west to get back in the starting room.

The third condition is important, because it is difficult to handle with directions such as "southeastup"

## Map legend
- "O" is a not marked room
- "Z" is the room, were you stay (or where the mapper was stopped)
- "B" is a room where you can go up or down (to an other layer)
- "R" is a room where you can go down (to a lower layer)
- "H" is a room where you can go up (to a higher layer)


## How can I use the ASCII-Mapper 2.0?
First of all, you have to install the evented navigation (https://github.com/Mundron/Evented-Navigation).
Next, you install "Mapper_script_part" and "Mapper_alias_part".
The "script part" contains the base code with all the nessesary functions.
But you use it ingame, you should create some alias which calls the mapper functions.
Therefore, the "alias part" is one possible way.
Here, I will just explain which functions can be used and you can check yourself how to apply them in the uploaded alias.



## ASCII Mapper Functions

You can see the mapper as a kind of object. If you like to use it, you can initialize it. But you can also stop it oder let it continue. When activated, the mapper will do the work on its own. You don't have to add the rooms or pathes.
Everytime you get into a new room, the mapper will update itself. This way, there will be no problem if you use accidently a nonexisting exit or if the exit is block by something. Moreover, the mapper will shoot down if it finds a contradiction to the conditions. If there is an exit like "southeastup", the mapper will give just a warning, but if you use a direction like this, the mapper will stop itself to prevent failures.
Nevertheless, you can add a few informations to the mapper.
For example, you can mark the room were you stay. The mark has to be a single number or a single letter. Later your mark will appear on the shown map.
Further, you can add a kind of checkpoint with an exit. If you use this exit at the checkpoint, the mapper will automatically stop and if you use an other exit at the checkpoint, it will automatically continue. This way, you can mark the start of a region and the mapper will just work inside the region. Naturally, you are allowed to add more than one exit at a checkpoint and more than one checkpoint.
If you start the client or if you like to use the mapper at different regions, you can save it in a text file too.

## --	ASCIIMapper:init()												 --
If you call this function, then the mapper will be newly initialized.
To start the mapper in a new region, you should call this function.
But be aware that the old map will be deleted!
If you want to keep it, you should save it!

## --	ASCIIMapper:markroom(s)										 --
This function will plug in the single number or single letter mark on the map.

## --	ASCIIMapper:addblocker(dir)								 --
Calling this function with a directions as the argument,
will save the current room as a checkpoint and add the direction as an exit.
If you leave this room through the saved exit, the mapper will automatically
stop and automatically continue if you use an other exit.
This way, you can mark a boundary of the region which you like to map.

## --	ASCIIMapper:deleteblocker(dir)						 --
Here, you can delete a point, which is saved in the function above.

## --	ASCIIMapper:deleteallblocker()						 --
Delete all checkpoints.

## --	ASCIIMapper:stop()
You can also manuelly stop the mapper if you like to do something else
or if you are finished.

## --	ASCIIMapper:continue()										 --
And you can activate the mapper again. BUT you have to stay in the same
room in which you stop the mapper!

## --	ASCIIMapper:showmap()											 --
Shows the whole map (small version) including all layers ingame.

## --	ASCIIMapper:showmapdoublesize()						 --
Shows the whole map (large version) including all layers ingame.

## --	ASCIIMapper:savemap(filename)							 --
Saves the map (small version) in a text file. Please check the path in the code! You can add the prefered save directory after import the package in the script item "Mapper core" in line 33.


## --	ASCIIMapper:savemapdoublesize(filename)		 --
Saves the map (small version) in a text file.  Please check the path in the code! You can add the prefered save directory after import the package in the script item "Mapper core" in line 33.
