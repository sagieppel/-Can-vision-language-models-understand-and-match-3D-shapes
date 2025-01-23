# Can Large vision language models understand and match 3D shapes: Generate images of same 3D object but with different orientations, textures/materials, and environments

This script will procedurally generate images of the same objects but made of different materials, viewed from different directions, orientations, and in different environments and illumination. The goal is  for testing Vision Language models (VLM) and humans ability to recognize  and match 3D shapes. For more details see the paper [Do large language vision models understand 3D shapes?](https://arxiv.org/pdf/2412.10908).

# What this generate
Given an input folder of 3d objects, for each object it will generate multiple images of the same 3d object but with different orientations, materials and backgrounds. Additional script used to turn this into a 3D object matching test.

![](/Figure1.jpg)
**Figure: Few examples for tests generated by script. Find objects of same 3d shapes but different rotation, texture, and background**



# What you need
## Hardware + Software
The script was run with Blender 4.3  with no add-ons, it can run with GPU or CPU but run much faster with a strong GPU (cuda enabled).

## CGI Assets  
 
3D Objects Folder, HDRI background folder, and a folder of PBR materials. Example folders are supplied as: “HDRI_BackGround”, “PBR_Materials”, and “objects”.
The script should run as is with these folders.

However, if you want to create truly diverse data, you need a large number of backgrounds, objects, and PBR materials. This can be downloaded for free at:
HDRI Backgrounds:[PolyHaven](https://polyhaven.com/).

PBR Materials: [Vastexture](https://sites.google.com/view/infinitexture/home).
 
3D  Objects: [Objaverse](https://objaverse.allenai.org/), [Shapenet](https://shapenet.org/). 


# How to use it.
There are two ways to use this code, one from within Blender and one from the command line.
To run from within blender, open DatasetGeneration.blend and run  main.py from within blender.

To run from the command line, use the line:
blender DatasetGeneration.blend --background -noaudio -P  main.py

Or sh Run.sh

In this case, all the run parameters will be in the main.py file.


*** Note all paths are set to the example folder supplied, running main.py from DatasetGeneration.blend or  Run.sh should allow the script to run out of the box.

Note for Blender4.3  the main.py you run from within Blender is stored inside the .blend file and is different from the main.py file in the code folder.
If you change one, the other will not change.
This can be very confusing. Blender python is very confusing in general.
Note that while running, Blender will be paralyzed and will not respond.



## Main run parameters

The main running parameters are in the main.py in the Input parameters section.
This include:

HDRI_BackGroundFolder = path to the HDRI background folder

ObjectFolder = Path to the folder containing the object files (for example, shape net folder)

OutFolder = path to output folder where generated dataset will be saved

pbr_folders  = path to a folder containing the PBRs textures subfolders

Sample folders to all of these assets folders are supplied with the code and could be used as reference.
In general, the code should run as is from the command line and from within Blender GUI.

Additional boolean parameters control which element will be modified in the image and which will remain constants (orientation,material,background). See input parameters section in main.py for more details.

# Post processing: cleaning and making tests
## Cleaning:
In some small fraction of the generated images the objects will too small to be visible. The script [Filter_images/Filter_Images.py](https://github.com/sagieppel/Can-vision-language-models-understand-and-match-3D-shapes/blob/main/Filter_images/Filter_Images.py) scan the output folder and filter these cases:

# Making tests:

To turn the generated images into a multi choice 3D shapes matching test for humans see script: [Make_Quiz/Human_Quiz/HumanQuiz.py](https://github.com/sagieppel/Can-vision-language-models-understand-and-match-3D-shapes/tree/main/Make_Quiz/Human_Quiz) this receives the generated images and turns them into tests for humans.

To turn the generated images into a multi choice 3D shapes matching test for Vision language models (VLM) see scripts in folder: [Make_Quiz/LVM_AI_QUIZ](https://github.com/sagieppel/Can-vision-language-models-understand-and-match-3D-shapes/tree/main/Make_Quiz/LVM_AI_QUIZ).
 
# Input folder structure:
See supplied sample folders for reference:

## Object Folder structure.
The object folder should contain  subfolders of objects divided by categories.

Script for downloading the Objaverse object and arranging them in the right format can be found in [handle_assets/Download_Objaverse_ByCat.py](https://github.com/sagieppel/Can-vision-language-models-understand-and-match-3D-shapes/blob/main/handle_assets/Download_Objaverse_ByCat.py)

## HDRI folder
This should just contain HDRI images for the background.

### PBR format
The PBR folder should contain subfolders, each containing PBR texture maps.
Blender read texture maps by their name. Therefore untypical map names will be ignored. The texture maps names should contains one of the following: "Color.","Roughness.","Normal.","Height.","Metallic.","AmbientOcclusion"r,"Specular.","Reflection","Glosinees". The PBR in the [Vastexture](https://sites.google.com/view/infinitexture/home) repository already comes with a standard name.

For PBR from other sources: The script: PBR_handling\StandartizePBR.py will automatically convert a set of PBR folders to standard PBR folders (mainly rename texture maps files to standard names)



# Dealing with blender slowing, memory  issues and crashes
In case you encounter blender Crash or slowing. To avoid the need to restart the program every time Blender crashes, use the shell script Run.sh. This script will run the blender file in a loop, so it will restart every time Blender crashes (and continue from the last set). This can be run from shell/cmd/terminal: using: sh Run.sh.
Also, in some cases the blender doesn't crash but  can start getting slower and slower, one way to solve it is to exit the blender once in  a while. Setting the parameter: use_priodical_exits
In the main.py to True, will cause Blender to exist every 10 sets. If this is run inside Run.sh blender will be immediately restarted and will start working cleanly.

# Example output images:
Sample for images generated by this method can be download from [Zenodo](https://zenodo.org/records/14681299), [Google drive](https://drive.google.com/drive/folders/1pxSnX-qpBfcQ47BbPQmy8pbURk0vXMzu?usp=drive_link), [Pcloud](https://e.pcloud.link/publink/show?code=kZz7FKZ8xfKSIHppBShSuU65cxBvQkorVXV).

# Notes:
1) Running this script should paralyze Blender until the script is done, which can take a while.
2) The script refers to materials nodes and will only run as part of the blender file.

