### How do I beat these guys?!
---

# Analysis

at this this chall we have two file .exe and .pck

![image](https://github.com/user-attachments/assets/cd7a32ad-b737-4582-8d38-3b4c53860c81)


with .exe file will take us to a game where we have to beat all enemies to get the flag. Of course, author may do something to make it impossible to win the game in the usual way.

and A .pck file is a package file — a binary or archive format used by different software to bundle data together.

I will use [Godot](https://godotengine.org/) to solve it , and I use Godot editor version 3.5 to edit   [Godot-ver3.5](https://godotengine.org/download/3.x/windows/)  i will create a tutorial file later.

![image](https://github.com/user-attachments/assets/4cfe4197-62b1-4f96-a3a5-4b29d51f83ca)

with godot you can edit logic code and run exe file right on it 

![image](https://github.com/user-attachments/assets/db79f44a-88e0-489c-853b-b43d686207f1)

Cuz the requirement is to defeat all enemies to get the flag, so i will pay attention on script file enemies.gd and some file related files, like you see code and filenames are quite clear

![image](https://github.com/user-attachments/assets/3d94e2da-ac2e-4552-9219-01af76afe7c2)

Godot use a programming language call [GDScript](https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_basics.html)  you can reasearch about it.

# Solution
---

![image](https://github.com/user-attachments/assets/d6c72328-cabe-41b1-8402-3b090dcd54f6)

at script file enemies.gd i found a `function alive` so i delete `alive` and ran the program right on godot `F5`

![image](https://github.com/user-attachments/assets/32631445-0cb1-4c57-961b-281aa52c8cb0)







