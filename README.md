# PyRosetta-DDG
Rosetta Commons: PyRosetta DDG calculation script for python3 (and how to get it working)


This is my process of getting PyRosetta to function on my Mac computer.
## Starting with Downloads
### These steps are a more detailed version of what is on https://www.pyrosetta.org/downloads.
1. Download Python and Anaconda (at the time of making this, python3.13 is the most current version and Anaconda runs python3.12). When downloading accept everything.
2. Download PyRosetta from https://www.pyrosetta.org/downloads . The links for the files to download are under "Latest PyRosetta Versions, Python and Python-wheel Packages" as East or West Coast and click one of those links. I used west coast and since I used Anaconda (conda) I downloaded the python3.12 version and my Mac has an M1 chip, so I chose the m1 file instead of mac. If you have an M1, M2, or M3 chip (this is what is available as of Jan 2025) choose the M1 file, otherwise, if the chip is intel in a mac choose the mac file and any other computers and chip versions get that version. Click the file you would like it will bring you to a new page and choose the latest version.
   a. I picked PyRosetta4.Release.python312.m1/, then PyRosetta4.Release.python312.m1.release-392.tar.bz2.
4. The download will take a while.

## Activation
### The website https://www.pyrosetta.org/downloads has details for this, but this is slightly more detailed.
#### In case you are new to coding, you need to press enter after each command.
1. In Terminal unpack the rosetta file. You will likely need to `cd` into your Downloads folder, or wherever the folder is downloaded to. Unpack with `tar -vjxf PyRosetta-<version>.tar.bz2`, so I inputted `tar -vjxf PyRosetta4.Release.python312.m1.release-392.tar.bz2`.
```
sk@MacBookPro ~ % cd Downloads/
sk@MacBookPro Downloads % tar -vjxf PyRosetta4.Release.python312.m1.release-392.tar.bz2
```
3. This will take some time.
4. Once complete, create a new conda environment with `conda create -n <name.for.environment> python=<version>`. Personally, I named my environment rosetta and since I downloaded the python3.12 version and conda works with python3.12 my command looked like  `conda create -n rosetta python=3.12`. Once it pauses at "Proceed ([y]/n)?", just put `y`.
   a. You can always change the Python version in the environment after it has been created in case you put the wrong one with `conda activate <name.for.environment>` then `conda install python=<version>`.
5. Activate the environment you created with `conda activate <name.for.environment>`. Mine is `conda activate rosetta`. 
6. Then `cd` into the unpacked folder (mine was PyRosetta4.Release.python312.m1.release-392) and then the setup folder within.
```
(rosetta)sk@MacBookPro Downloads % cd PyRosetta4.Release.python312.m1.release-392/setup/
```
7. Once, within setup command `python setup.py install`. This should also take a while.
8. Then type `python` you should get the >>> then do the `import pyrosetta;pyrosetta.init()`
9. There will likely be a security pop-up, at least on Macs, I can't remember what the exact buttons are, but I think it was the one on the left that I pressed. Essentially, choose the one that ignores the security issue.
10. You then need to go to settings and in Privacy and Security, scroll down to security there will be something about rosetta and allow it anyway.
11. Back in Terminal, if Python is still running, you'll see the >>>, `exit()`.
12. Repeat step 8 and that should get you
```
┌──────────────────────────────────────────────────────────────────────────────┐
│                                 PyRosetta-4                                  │
│              Created in JHU by Sergey Lyskov and PyRosetta Team              │
│              (C) Copyright Rosetta Commons Member Institutions               │
│                                                                              │
│ NOTE: USE OF PyRosetta FOR COMMERCIAL PURPOSES REQUIRE PURCHASE OF A LICENSE │
│         See LICENSE.PyRosetta.md or email license@uw.edu for details         │
└──────────────────────────────────────────────────────────────────────────────┘
PyRosetta-4 2025 [Rosetta PyRosetta4.Release.python312.m1 2025.04+release.8ef4ef1eaaf6596709534135ef10420829ddc9d6 2025-01-21T15:59:35] retrieved from: http://www.pyrosetta.org
core.init: Checking for fconfig files in pwd and ./rosetta/flags
core.init: Rosetta version: PyRosetta4.Release.python312.m1 r392 2025.04+release.8ef4ef1eaa 8ef4ef1eaaf6596709534135ef10420829ddc9d6 http://www.pyrosetta.org 2025-01-21T15:59:35
core.init: Rosetta extras: []
basic.random.init_random_generator: 'RNG device' seed mode, using '/dev/urandom', seed=1043425028 seed_offset=0 real_seed=1043425028
basic.random.init_random_generator: RandomGenerator:init: Normal mode, seed=1043425028 RG_type=mt19937
```
13. This means everything is working. If you get an error through any of this ChatGPT was incredibly helpful.

## Make a working python3 script for delta_score_per_mutation.py
### Way 1
1. Make sure you are not actively in Python, no >>> in front of your cursor, and you are in the setup folder within the PyRosetta folder and the conda environment is running.
```
(rosetta)sk@MacBookPro setup %
```
2. Then create a new .py file. The name can be anything you want I chose ddg_mutation_scan
```
(rosetta)sk@MacBookPro setup % nano ddg_mutation scan.py
```
3. This will get you to a blank text editor for a Python script file.
4. Copy the delta_score_per_mutation.py script from within the app folder of the PyRosetta folder into ChaptGPT and obtain a python3 version. Copy and paste that script into the text editor. The original is copyrighted and available only under a license. I am not making it available here, sorry. 
5. Then exit ^X (control x) then ^Y (control y) for yes to save then enter.
6. This file should then be within your setup folder.

### Way 2
1. Within Finder (on Macs), find the PyRosetta folder open it find the app folder and delta_score_per_mutation.py should be in it. I would make a copy of it, but make sure that the file's name does not contain a space or period before the ".py". Rename as delta_score_per_mutation2.py or something. Put the copy into the setup folder.
2. Open the copied delta_score_per_mutation.py file, and delete everything in it.
3. Follow step 4 of Way 1. In this case, the text editor is the cleared open copied delta_score_per_mutation.py file and just paste the script into it. 
4. Then exit the file and save when the window asking if you want to save pops up.


## Run the DDG script
1. Download a .pdb file of your desired protein and put it in the same setup folder that we have been working in. Make sure that the .pdb only contains the protein structure and not other molecules that interact with it.
2. Then run `python <script_name>.py <protein>.pdb --minimize`. Mine looked like:
```
(rosetta)sk@MacBookPro setup % python ddg_mutation_scan.py ADE1.pdb --minimize
```
3. It will take a while to get results and is entirely dependent on the length of the protein.
4. The program will create a .csv file within the setup folder when it is done.

## Troubleshooting
1. I put files that contained the following codes into the setup folder in case they were unreachable by the program from being in a different folder:
`pyrosetta.init()`, `pyrosetta.pose_from_file()`, `pose`, `pyrosetta.get_fa_scorefxn()`, `TaskFactory`, `pyrosetta.rosetta.protocols.minimization_packing.MinMover`, `FastRelax`, `MutateResidue`, and `pyrosetta.rosetta.protocols.simple_moves.MutateResidue`.
  Simply any file within the overall PyRosetta folder that contained the above coding or a significant portion of the code was copied into the setup folder.
2. Ask ChatGPT it will eventually get you the help that you need.






