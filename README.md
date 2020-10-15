# Context 

Searching through vast libraries of sound samples can be a daunting and time-consuming task. 
Modern audio sample browsers use mappings between acoustic properties and visual attributes to visually differentiate displayed items. 
There are few studies focused on the effect of these mappings on the time it takes to search for a specific sample. 
We designed a study to compare shape, colour and texture as visual labels in a known-item search task.

We are specifically interested in visually differentiating the **timbre** of sound samples so we used samples  of different instruments all playing the same note (see the data section).


# Overview
This repository contains source code for a browser-based study on audio visualisation. The study is bi-lingual and participants can choose between french or english as their preferrred language.
The goal of the study is to determine whether different sound visualisation approaches can assist in a known-item search task.

Study participants complete a series and tasks and questionnaires.

Tasks proceed in this manner:
* Participants  are presented with a subset of samples on a cavas and a target sound.
* They can press the space-bar to listen to the sound that they must find. 
* They must find and click on the visual element representing the the sound.
* They position their mouse cursor to highlight and listen to sound samples.

![Study interface](https://github.com/nerrull/timbre-study/raw/master/readme_image/interface.png)

We test 3 different sound visualisation methods. 
This study is designed to produce data that will allow us measure the impact of the visualisation method on the participants ability to complete the task.
It also serves as a way to measure whether providing positionl information can assist in the search task,
as it allows for precalculated canvas coordinates to be assigned to the sound samples. 
We use a dimensionality-reduction approach on timbral profiles extracted using [this filterbank](https://github.com/nerrull/ERBlet-Cochlear-Filterbank/).

### Shape
Visual elements have a contour that is based on the temporal envelope of the sound sample.

### Colour
Visual elements have a colour that is based on timbral descriptors.
* The normalized spectral centroid (correlated with timbral brightness) is mapped to a blue-red gradient
* The spectral flatnesss is mapped to the color saturation

### Texture
Each sound sample is associated with a textural image file. We generate these textures using a custom-built tool you can find [here](https://github.com/nerrull/timbre-texture-generator).

![Visualization modes](https://github.com/nerrull/timbre-study/raw/master/readme_image/vis_modes.png)

The study tasks are divided into 4 parts:

#### Phase 1 : Learning to look for sounds
#### Phase 2 : Learning the visualisation strategy
#### Phase 3 : Using the visualisation strategy
#### Phase 4 : Measuring the baseline

# Article
For more in-depth information you can consult our article published in the 
proceedings of [Audio Mostly 2019](https://audiomostly.com/).

Please cite this article if you use our study in your own research.

```REFERENCE PENDING```

# Set up
Install and launch JATOS on your machine, I recommend following the official guide:
http://www.jatos.org/Get-started.html

It is much simpler to import a study using the jatos interface so we suggest the following procedure

1. Download a zipped study (this is the easiest way to add a study to JATOS) from [this Google Drive folder](https://drive.google.com/drive/folders/1gR94POzofFTQ77MwG61Jut5nZVMyvCDy?usp=sharing) .
2. On the JATOS homepage, click ''Import study'' and find the zipped package.

We have created several versions of the study for different audiences. The source code for each variant is its own sub-branch of this repo.
The zipped packages already contain the git files to link them to the appropriate branch.

## Variants (branches)
### master
Participants are assigned a random visualisation mode and complete one full pass of the study.

### students
Same as master, but participants can choose the visualisation mode and the final page presents some of the data collected (mean times, number of samples visited).

### on-site
This variant was developed for participants who were paid to complete the study on-location. 
They essentially complete the study 3 times, once for each visualisation mode. 
At the beginning of the study, they much choose a number which represents a specific ordering of the task modes.

# Modifying the study
You can access the source code in ```[your JATOS installation folder]/study_assets_root/audio_texture```.
Jatos uses a modular approach to building studies. The code for each  module of the study is in the root folder.

### Important: Please modify the consent form if you use this code to run your study
The text for this form is in ```[root]/static/html/consent.html```.
In general, almost all of the text used in the study is in ```[root]/static/html/```.

### Task generation
The logic for selecting samples and generating sets of tasks can be found in ```assign_task.html```

### Data 
For the study to be functional, you need audio samples, texture images and a dataset containing sample ids, coordinates, colors and audio envelopes. 
The study packages contains the dataset we used for our study. You can also download the sample dataset from [this link](https://drive.google.com/drive/folders/1gR94POzofFTQ77MwG61Jut5nZVMyvCDy?usp=sharing). Unzip the contents into ```[study source folder]/static/```. 

We are specifically interested in visually differentiating the **timbre** of sound samples.
This dataset uses samples from the [nSynth dataset](https://magenta.tensorflow.org/datasets/nsynth) and contains ~800 samples of various instruments all playing midi note 64 with 75 velocity.

Samples are identified by an index number and a name, and their associated files are named accordingly.
The dataset is a .json file with the following structure:
```
{
    n_samples: int,
    samples: array[
        {
            index :int,
            coords: array[(float x, float y)],
            env: array[float],
            name: string,
            color: array[float r, float g, float b]
        }]
}
```
*  ```name``` is used to look up the audio and image files
* ```env``` is used to generate the contour in the SHAPE visualisation mode
* ```coords``` are normalized coordinates for displaying on the canvas, should be in range [0,1]
* ```color``` is (r,g,b) values for the COLOR visualisation mode, should be in range [0,1]
* Images should be in .jpg format
* Audio should be in .mp3 format

### Frameworks used
We use [jsPsych](www.jspsych.org) to run multiple iterations of a task within a module and for its timing functionalities. 
To modify this behaviour look at ```audio-image-space.js```.
 
We use [p5.js](p5js.org) for the interactive canvas. All processing code is in ```utils/processing_sketches.js```

We use [survey.js](https://surveyjs.io/) for the survey portions of the study.

## Questions and documentaion
For the moment, the code is not very well-documented.
If you have any specific questions about using this project, feel free to open up an issue.


# Citation
If you use this work in your own research please cite our Audio Mostly article.

```REFERENCE PENDING```

as well as

## JATOS
>Lange K, Kühn S, Filevich E (2015) "Just Another Tool for Online Studies” (JATOS): An Easy Solution for Setup and Management of Web Servers Supporting Online Studies. PLOS ONE 10(6): e0130834. https://doi.org/10.1371/journal.pone.0130834


If you use samples from the nSynth dataset you should cite:
>Jesse Engel, Cinjon Resnick, Adam Roberts, Sander Dieleman, Douglas Eck, 
Karen Simonyan, and Mohammad Norouzi. "Neural Audio Synthesis of Musical Notes 
with WaveNet Autoencoders." 2017.







