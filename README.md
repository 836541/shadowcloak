# AV-ExtensionsMTD
MTD-Based Ransomware Prevention using new extensions and file associations for files.


Moving Target Defense for Ransomware Prevention

------------------------------
[1] What is this tool ? 
------------------------------

A Proof of Concept tool based on the article "Ransomware protection using the Moving Target Defense"(SUHYEON, L. & HUY, K. K. & KYOUNGGON, K., 2019)
The tool changes the extensions of files in the file system for new pseudo random crypto secure ones.
After that, the new extension is added to the registry. 
For example, if the tool chooses ".yuni" to substitute ".pdf", then all files from a given directory 
that has the suffix ".pdf" will become ".yuni" and will open like a ".pdf" file. It's important to say
that ".pdf" will continue to open normally.
It exploits the fact that most ransomware searches for files using an extensions whitelist, which makes
ransomware unable to find protected extensions in a system using this tool.

---------------------
[2] Cons 
---------------------

(A) Ransomware can bypass if they start to use a blacklist of extensions instead of whitelists.

(B) Ransomware that encrypt using lists of directories can bypass too. However, this type of Ransomware
is more fragile against Decoy/Deception solutions.

(C) Files with extensions that work only in your environment aren't too shareable. However, the tool has
a option which allows you to choose directories where all files are renamed to the standard extensions again.

(D) Ransomware can bypass if they start using whitelist of file formats (magic bytes). However, this 
takes more CPU power and a module that hides file formats is possible.

So, because of the cons, this tool isn't helpful against Advanced Persistent Threats (APTs), but useful against Ransomwares that dont know about the tool existence.
-------------------------------
[3] PARAMETERS - PLEASE READ
-------------------------------
This tool has SEVEN parameters (TWO of them are optional).

[-m/--mtd=]
if True: change from standard extensions (.pdf, .mp4...) to new random extensions.
After the change is made, an output file will be generated containing all the old-new extensions relation
so you can use it later for reverting changes. Let's call that file as OUTPUT.TXT in the documentation (however it generates a file with 4 letters filenames and no extension).
if False: change from the new random extensions to standard (.pdf, .mp4...) extensions

[-d/--directory]
(file.txt): Input here a txt file containing the directories you want to change the files.
Syntax is a sequence of directories which in the end of each one there's a comma (including the last)
Example:
C:\Users, C:\Windows\system32 , C:\Program Files,
(if --directory = None): The tool will use "C:\".

[-s/--subdirs ]
(if True): recursive search of files in the directories. For example, a recursive search in C:\ will list
every single file of the disk volume.
(if False): not a recursive search.

[-w/--whitelist]
(whitelist.txt): A txt file containing all the directories that shall not have the files listed.
Each item shall end with a comma.
Useful if you want to do a recursive search but dont want certain directories to be affected.
You can also write filenames here.
Example:
C:\Users\Admin\Desktop, C:\Windows , valorant.exe , 
(if --whitelist = None): No whitelis will be used.

[-r/--registry] (only works if --mtd = True)
(if True): after changing the extension, each new extension will also be added to registry to create new file associations.
(if False): only rename the extensions. 

[-l/--loop] 
(if True): the software will run forever, so it will try to rename each new file that appears in the 
choosen directory.
(if False): the software will run just one time. 
if --registry = True, --loop is set to False.
LOOP RUNS MUST HAVE THE OUTPUT.TXT FILE AS -directory ARGUMENT

[-e/--extension]: 
(extensions.txt) Input here a file where the extensions you want to protect are written.
The syntax of the file is a sequences of extensions followed by commas (including the last extension of the sequence)
Example is a txt file with:
extension 1, extension2 ... extension x , 
.pdf, .mp4 , .docx,
The tool has a standard extension txt file (using WannaCry whitelist) which you can use or observe the syntax to create your own.

--------------------------------
[4] HOW TO USE (READ PLEASE)
--------------------------------

START: Create your txts for extensions (or use the standard), directories and whitelist.

(A): Make a ONE TIME RUN with 
--mtd= True    --registry= True   --loop= False
This will rename all files from the choosen directories, create new file associations in registry 
and will output a file containing all the relations between old extensions and new extensions. Lets call
that file as OUTPUT.TXT.

(B): Set a LOOP RUN with 
--mtd = True   --loop = True (which makes --registry = False)
--extension = OUTPUT.TXT (IMPORTANT) 
So all new files that appear on the choosen directories will be auto renamed.

(C): Set a LOOP RUN with 
--mtd = False   --loop= True  -- extension = OUTPUT.TXT
This is like (B) but with -mtd = False. So, the point of (C) is inputing --directory a sequence of 
directories which you want to make all files there be auto renamed to standard extensions.
The point of (C) is having directories which you can drag your files to make them instantly "normal" again so you can share them online with  usual extensions.

-----------------------------------
[5] FUTURE IMPROVEMENTS
-----------------------------------

(A): A few new extensions may not associate with a file type. The reason for that is because I didnt find a way yet to transfer all values from a registry subkey
like "HKEY_CLASSES_ROOT\\.pdf" to the new one which is going to "replace" it (not really replace because both will still work). This script only transfer the first value it encounters. However, almost always the first value is enough. It's very rare, then when you try to open a file and it dont find its find association you can do it manually ("open with" GUI option) without much effort since fails are unusual.

(B): File type (magic bytes) MTD module.

(C): More readable code (this software has some ugly variables).


