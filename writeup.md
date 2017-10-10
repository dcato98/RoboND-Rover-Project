# Udacity Robotics Nanodegree
---

## Project #2: Search and Sample Return

---


**Goals of the project:**
* Detect navigable terrain, obstacles, and samples of interest (golden rocks)
* Use perspective transform, color thresholds, and coordinate transforms to build an image processing pipeline that converts local rover images to a global map (`process_image()` function)
* Produce a video of the mapping using a test dataset of rover images
* Complete the `perception_step()` and `decision_step()` functions to create a map, update `Rover()` data, and decide how to issue throttle, brake and steering commands
* Meet the accuracy specs outlined in the project rubric 

[//]: # (Image References)

[image1]: ./output/output_8_1.png
[image2]: ./output/output_13_1.png
[image4]: ./calibration_images/example_rock_binary.jpg

[video1]: ./output/test_mapping.mp4
[video2]: ./output/test_mapping_2.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points

---
### Writeup

#### 1. Provide a writeup that includes all the rubric points and how you addressed each one.

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images. Add/modify functions to allow for color selection of obstacles and rock samples.

In cell 5 of the project [notebook](Rover_Project_Test_Notebook.ipynb), I modified `perspect_transform` to additionally return a binary mask which captures the line of sight of the rover. 

![alt text][image1]

In cell 8, I added a function, `find_rocks`, to identify rock pixels given an image.

![alt text][image2]

#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 

See output video cells 12 and 14 in the project [notebook](Rover_Project_Test_Notebook.ipynb).

![Test Video 1][video1]
![Test Video 2][video2]

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

See files [perception.py](code/perception.py) and [decision.py](code/decision.py).

The `perception_step()` function is nearly identitical to `process_image()` in the notebook after modifiying the data source from the `Databucket()` object to the `Rover` object. One slight modification made in perception.py was to add 10 and 1 units per navigable pixel and obstacle pixel, respectively, instead of taking navigable pixels as the ground-truth. This substantially increased the mapping fidelity.

No changes were made to `decision_step()`.

#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

Note - I used screen resolution: 800x600, graphics quality: Fastest (~22-23 FPS)

The rover is now able to successfully and autonomously map the world.

In `decision_step()`, I experimented with adding a bias to `Rover.steer` in an attempt to hug the right wall and reduce the chances of getting stuck in a loop, but this didn't successfully solve the problem.

The rover still gets stuck in driving in circles from time-to-time, and also occasionally goes down a previously discovered path instead of searching for new ones. So, the next improvement to the rover would first be to implement 'hug the wall' mode, in order to map the world boundaries. An even further improvement would be to add a search-mode (probably A\*) to map unknown spaces more efficiently.

The image processing pipeline is highly tuned to the flat, color-specific terrain and would certainly need to be retuned for other environments. In an environment with hills, it is likely to fail outright at building accurate maps. These would be good places to improve the image pipeline.
