---
title: "Habit Task Development"
collection: portfolio
excerpt: "<div style='display: flex; align-items: center;'>
    <img src='/images/habitTask.png' width='400' height='400' alt='habit task' style='margin-right: 10px;'>
    <p style='text-align: justify;'>
    This project involves running an interactive experiment using Psychtoolbox, designed to study decision-making and task performance in a controlled environment. Participants are presented with a series of instructions and tasks, including choosing between treasure chests, each associated with different probabilities of containing treasure. The experiment tracks responses, timing, and behavioral outcomes through various event sequences, and the data is logged and saved for analysis. The setup includes detailed visual presentations, including animated sprites and dynamic feedback, all displayed on a multi-screen setup for enhanced user experience. Data from each trial is meticulously recorded, including timing and event outcomes, and saved for further analysis, ensuring that no previous data is overwritten. The project aims to collect comprehensive behavioral data to investigate cognitive processes in decision-making and learning, with a focus on efficient task management and precise timing.
</p>
</div> "
permalink: /portfolio/habit
date: 2024-09-15
layout: archive
---

### Project Lead
Shane D. McKeon

### Faculty Lead
William Foran

### Project Start Date
July 2024

### Current Project Status
Completed 

### Github Repository
https://github.com/LabNeuroCogDevel/choice-landscape

## Code Documentation

Breifly, the MATLAB script ([habit.m](https://github.com/LabNeuroCogDevel/choice-landscape/blob/master/habit.m)) defines a function designed to run a habit-formation experiment using Psychtoolbox. The function starts by closing any open screens and standardizing keyboard input naming conventions. It then loads the experimental system and event timing parameters from input arguments, sets up the display screen, positions graphical elements, and loads textures for visual stimuli. After initializing the system, it presents instructions to the participant and records the start time of the experiment.

To ensure data integrity, the function sets up an output directory and checks if a previous data file for the given patientID exists. If a file is found, it is renamed with a timestamp backup before new data is logged. A diary function is used to record all console output for debugging and documentation purposes. The function then initializes an empty structure, record, to store trial data and logs the experiment start time.

During the experiment, it iterates through predefined events, determining event onsets dynamically—setting the first event or inter-stimulus intervals (isi) to start immediately, while others follow the duration of the previous event. Each event executes a corresponding function, which updates the record with event names, outputs, and timestamps. The function continuously saves the experiment state to a .mat file to prevent data loss. Finally, the experiment concludes by closing all open resources using the closedown function.

### Loading event timing
This MATLAB function, [load_events](https://github.com/LabNeuroCogDevel/choice-landscape/blob/master/private/load_events.m), generates a structured timeline (timing) for a multi-block experiment where participants make choices between different options. The experiment consists of four blocks, each with a predefined number of trials (36, 36, 72, and 72) and different probabilities associated with selecting each option.

For each block, the function generates trial events. At the beginning of each trial, two different options are randomly selected from "left," "up," and "right." The trial sequence follows a structured order:

**Choice Event** – The participant selects between two options. The choice event duration is set to 2 seconds, with choice probabilities varying by block. <br>
**Inter-stimulus Interval (ISI)** – A brief pause of 0.52 seconds follows, during which a blue fixation cross is displayed, and the character moves. <br>
**Feedback Event** – Feedback is provided for another 0.52 seconds. <br>
**Inter-trial Interval (ITI)** – A 1-second pause with a white fixation cross precedes the next trial. <br>

The function dynamically calculates event onset times so that each event starts immediately after the previous one ends. To ensure correct execution, it also includes commented-out debugging assertions that verify correct probability distributions and trial transitions.

### Setting up the default screen

The [setup_screen](https://github.com/LabNeuroCogDevel/choice-landscape/blob/master/private/setup_screen.m) MATLAB code defines two functions, setup_screen and testscreen, which are used to set up a visual display screen for a task, likely related to a psychological experiment using Psychtoolbox.

setup_screen function: <br>
This function first prints a message indicating the loading of the screen (# loading screen).
It checks for the existence of a directory (/home/abel/matlabTasks/taskHelperFunctions/) that contains helper functions for the task. If the directory is found, the initializeScreen function from this helper directory is called to set up the screen, and a message is displayed indicating that the second screen is connected. If the helper directory is not found, the function defaults to using testscreen to set up the screen and prints a warning message. The Screen('BlendFunction', ...) line sets the blending function for rendering transparent textures using an alpha channel.
<br>

testscreen function:<br>
This function is used to initialize a screen for the task. It first sets the background color (bg = [254 227 180]), and then detects the available screens using Screen('Screens'). The screenNumber is set to the highest available screen index, which likely corresponds to a second monitor (if available). The function retrieves the dimensions of the selected screen (rect = Screen('Rect', screenNumber)) and opens a window on that screen using Screen('OpenWindow') with the specified background color.

In summary, these functions are used to configure a screen for presenting stimuli in an experiment, using a second monitor if available, and managing transparency settings for visual elements.

### Define the Characters Positioning 
This function, [setup_pos.m](https://github.com/LabNeuroCogDevel/choice-landscape/blob/master/private/setup_pos.m), defines the positions for the character on the screen, specifically for the different locations where stimuli (such as the choices of the treasure chest) will be displayed during the experiment. It uses the screen dimensions to calculate relative positions on the screen.

### Loading the textures 
The function [load_textures.m](https://github.com/LabNeuroCogDevel/choice-landscape/blob/master/private/load_textures.m) loads image files into a structure of named textures to be used within a Psychtoolbox experiment. Here's a detailed explanation of its components:
<br>

Loading Images: <br>
The function first searches for .png files in the out/imgs/ directory using the dir('out/imgs/*.png') command. Each image is processed to extract the filename (removing hyphens and replacing them with underscores) and load the image using imread. The image and its associated alpha channel (transparency) are stored in the variable img.
<br>

Handling Specific Textures: <br>
Ocean Bottom Texture: <br>
If the image's filename is ocean_bottom, it resizes the image to fit the screen's dimensions. The image is resized using the screen height and width, and if necessary, the image's alpha channel is adjusted to ensure proper transparency.
<br>

Handling Sprite Images:<br>
The function differentiates between different types of images:<br>
Avatar Sprites: Images like 'astronaut', 'shark', etc., are split into smaller sub-images (sprite frames) within the original image. Each sprite is expected to be arranged in a 4x4 grid. The image is divided into a grid, and each frame is converted into a texture using Screen('MakeTexture'). These textures are stored in a 4x4 cell array, and each sprite type is saved in tex_struct with the filename as the field name.
<br>

Well Sprites: Images like 'chest_sprites' are assumed to have a 7x2 grid. Similarly, the image is divided into smaller sections corresponding to each sprite, and these textures are stored in a 7x2 cell array.
<br>

General Textures:<br>
For other images that don't fall into the avatar or well sprite categories, the image is simply converted into a texture using Screen('MakeTexture') and stored in the tex_struct with the filename as the field name.

Output Structure:<br>
The function returns a structure tex_struct where the field names correspond to the image filenames, and the values are the corresponding textures (either individual textures or cell arrays of textures).
<br>

Key Notes:<br>
Image Resizing: The ocean_bottom texture is resized to the screen dimensions to ensure it fits properly during the experiment.<br>
Texture Handling: The function handles both individual textures (e.g., single images) and sprite sheets (e.g., grids of multiple images), converting them into textures suitable for Psychtoolbox.<br>
Alpha Handling: Transparency is managed by adjusting the alpha channel to ensure that black pixels are treated as transparent where necessary.<br>
Efficient Texture Management: By organizing sprite textures into grids, the function allows easy access to individual frames for animated sprites or well sprites.

### Instructions 
The function [instructions.m](https://github.com/LabNeuroCogDevel/choice-landscape/blob/master/private/instructions.m) is used to display a series of instructional screens for participants in a Psychtoolbox experiment. The instructions guide participants through the task by explaining what they will need to do and how the interface works. Here's a breakdown of the steps involved in the function:
<br>

Background and Setup: The function first initializes a few parameters, including the size of the chest images (chest_w, chest_h), which are used later for positioning the chest textures.
<br>

Instruction 1: The first instruction introduces the task. The background (ocean) is shown, followed by three chests placed at different screen positions (left, up, right). The instruction text tells participants that they will be looking for treasure in these chests. The screen waits for a key press before continuing.
<br>

Instruction 2: The second instruction builds on the first one. Two keys appear in front of two of the chests, and the participant is informed that they can choose between the two chests using the keys. The screen waits for a key press before continuing.
<br>

Instruction 2.5: This step further clarifies the mapping of the buttons to chest positions. The instruction informs participants which buttons correspond to the left, up, and right positions. The screen waits for a key press before continuing.
<br>

Instruction 2.7: The instruction emphasizes the time constraint, telling participants that they only have 2 seconds to choose a chest. The screen waits for a key press before continuing.
<br>

Instruction 3: The third instruction introduces the concept that the odds of a chest containing treasure will vary. Participants are informed that their task is to figure out which chest is most likely to contain treasure. The screen waits for a key press before continuing.
<br>

Instruction 4: The fourth instruction introduces a green bar at the bottom of the screen, which indicates how much longer the task will last.
The screen waits for a key press before continuing.
<br>

Instruction 5: The final instruction tells the participant to press any key twice to start the task. The screen waits for a key press before continuing.
<br>

Task Start: After all the instructions, the function records the onset time (onset = Screen('Flip', system.w, 0)) and waits for a final key press to officially start the task.
<br>

Key Aspects of the Function:<br>
Texture Drawing: The function uses the Screen('DrawTexture') command to display different images and textures on the screen (e.g., ocean background, chests, keys).<br>
Button Mapping: The function communicates which keys correspond to actions (e.g., choosing a chest) and positions on the screen.<br>
Wait for Key Press: The loop while k == 0 ensures that each instruction waits for a key press before advancing.<br>
Screen Flipping: Screen('Flip') is used to update the display and ensure that the content is shown after each instruction.<br>

Considerations:<br>
Screen Positioning: The chest and key textures are positioned using the system.pos structure, which stores the screen coordinates of the left, up, and right positions for the chests.<br>
Color and Text: The text is drawn in white ([255, 255, 255]) on the screen using DrawFormattedText.<br>

### Running through the events 
The habit function then captures the start time using GetSecs() and stores it as a string in str_starttime. The script then sets the output directory to a hardcoded path ('/home/abel_lab/luna_habit/results/patientResults/'). If the folder doesn’t exist, it is created. Next, the script checks for an existing .mat file for the subject, identified by patientID. If such a file already exists, it renames it by appending the current start time to avoid overwriting previous data.

The script then proceeds to collect data and log events. It iterates through each timing event, adjusting the onset time based on whether the event is the first in the sequence or a specific event (like an ISI). For each event, it calls a function associated with the event, stores the outcome, and logs the onset time. After processing all events, the script saves the results in the .mat file, storing both the system information and the event records.

