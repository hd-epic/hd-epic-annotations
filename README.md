# ![logo](logo-white.png) HD-EPIC: A Highly-Detailed Egocentric Video Dataset

<!-- start badges -->
[![arXiv-xx](https://img.shields.io/badge/arXiv-xx-green.svg)](https://arxiv.org/abs/2006.13256)
<!-- end badges -->

## Project Webpage
Dataset - download and further information is available from [Project Webpage](https://hd-epic.github.io/)
Paper is available at [ArXiv](https://hd-epic.github.io/)

## Citing
When using the dataset, kindly reference:
```
@article{perrett2025hdepic,
    author    = {Perrett, Toby and Darkhalil, Ahmad and Sinha, Saptarshi and Emara, Omar and Pollard, Sam and Parida, Kranti and Liu, Kaiting and Gatti, Prajwal and Bansal, Siddhant and Flanagan, Kevin and Chalk, Jacob and Zhu, Zhifan and Guerrier, Rhodri and Abdelazim, Fahd and Zhu, Bin and Moltisanti, Davide and Wray, Michael and Doughty, Hazel and Damen, Dima},
    title     = {HD-EPIC: A Highly-Detailed Egocentric Video Dataset},
    journal   = {arXiv preprint},
    volume    = {arXiv:2502.XXXXX},
    year      = {2025},
    month     = {February},
} 
```

## Narrations and Action Segments

This folder contains narration annotations structured as follows:

- `HD_EPIC_Narrations.pkl`: labels narration/action segments and associated annotations.
- `HD_EPIC_verb_classes.csv`: labels verb clusters.
- `HD_EPIC_noun_classes.csv`: labels noun clusters.

Details about each file are provided below.

### `HD_EPIC_Narrations.pkl`

This pickle file contains the action descriptions for HD-EPIC and contains 16 columns:

| Column Name           | Type    | Example                                                                             | Description                                                                           |
| --------------------- | ------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `unique_narration_id` | string  | `P01-20240202-110250-1`                                                             | Unique ID for the narration/action as a string with participant ID, video ID, and action index. |
| `participant_id`      | string  | `P01`                                                                               | ID of the participant (unique per participant).                                       |
| `video_id`            | string  | `P01-20240202-110250`                                                               | ID of the video where the action originated from (unique per video).                  |
| `narration`           | string  | `Open the upper cupboard by holding the handle of the cupboard with the left hand.` | Narration or description of the performed action.                                     |
| `start_timestamp`     | float64 | `7.44`                                                                              | Narration/action segment start time in seconds.                                                 |
| `end_timestamp`       | float64 | `8.75`                                                                              | Narration/action segment end time in seconds.                                                   |
| `nouns`               | list  | `['upper cupboard', 'handle of cupboard']`                                          | List of nounds extracted from the narration description.                                 |
| `verbs`               | list  | `['open', 'hold']`                                                                  | List of verbs extracted from the narration description.                                  |
| `pairs`               | list  | `[('open', 'upper cupboard'), ('hold', 'handle of cupboard')]`                      | List of (verb, noun) pairs extracted from the narration description.                     |
| `main_actions`        | list  | `[('open', 'upper cupboard')]`                                                      | List of main actions classes performed.                                               |
| `verb_classes`        | list  | `[3, 34]`                                                                           | Numeric labels for extracted verbs.                                                   |
| `noun_classes`        | list  | `[3, 3]`                                                                            | Numeric labels for extracted nouns.                                                   |
| `pair_classes`        | list  | `[(3, 3), (34, 3)]`                                                                 | Numeric labels for extracted verb-noun pairs.                                         |
| `main_action_classes` | list  | `[(3, 3)]`                                                                          | Numeric labels for main action categories.                                            |
| `hands`               | list  | `['left hand']`                                                                     | List of hands (`left hand`, `right hand`, `both hands`) mentioned in the narration. |
| `narration_timestamp`  | float64 | `8.0`                                                                               | Timestamp when the narration was recorded by the participant, in seconds.                                   |

### `HD_EPIC_noun_classes.csv`

This file contains information of nouns extracted from narration descriptions in HD-EPIC and contains 4 columns:

| Column Name | Type   | Example                                             | Description                                     |
| ----------- | ------ | --------------------------------------------------- | ----------------------------------------------- |
| `ID`        | int    | `0`                                                 | Numerical label assigned to the noun.           |
| `Key`       | string | `tap`                                               | Base form label for the noun.                   |
| `Instances` | list   | `['tap', 'tap:water', 'water:tap', ...` | List of parsed variations mapped to this label. |
| `Category`  | string | `appliances`                                        | High-level taxonomic category of the noun.      |


### `HD_EPIC_verb_classes.csv`

This file contains information of verbs extracted from narration descriptions in HD-EPIC and contains 4 columns:

| Column Name | Type   | Example                                             | Description                                     |
| ----------- | ------ | --------------------------------------------------- | ----------------------------------------------- |
| `ID`        | int    | `0`                                                 | Numerical label assigned to the verb.           |
| `Key`       | string | `take`                                              | Base form label for the verb.                   |
| `Instances` | list   | `['collect-from', 'collect-into', 'draw', ...` | List of parsed variations mapped to this label. |
| `Category`  | string | `retrieve`                                          | High-level taxonomic category of the verb.      |


## Digital Twin: Scene & Object Movements
We annotate object movements by labeling Temporal segments from pick-up to placement and 2D bounding boxes at movement onset and end. Tracks include even slight shifts/pushes, ensuring full coverage of movements. Every object movement is annotated and assgin to a scene fixture, providing a rich dataset for analysis. 
The annotation data is stored as a JSON object where each key represents the video name, and the value is a list of track annotations including 3D locations, bounding boxes, and assigned fixtures. Each track annotation corresponds to a unique object track within a specific time segment.

### Field Descriptions
You can access the data from: `scene-and-object-movements/hd-epic-scene-and-object-movements.json`. Below is the template format of the annotations which will be explained next.

**Structure**:

```jsonc
{
  "video_id": [
    {
      "id": "string",
      "time_segment": [start_time, end_time],
      "track_num": integer,
      "locations": {
        "start": {
          "frame": integer,
          "3d_location": [x, y, z],
          "bbox": [xmin, ymin, xmax, ymax],
          "fixture": "string"
        },
        "end": {
          "frame": integer,
          "3d_location": [x, y, z],
          "bbox": [xmin, ymin, xmax, ymax],
          "fixture": "string"
        },
        "other_frames": []
      }
    }
  ]
}
```


**Field Descriptions**:
- **`video_id`**: A unique identifier for the video ID, i.e. `P01-20240202-110250`.
- **`id`**: A unique identifier for the track, i.e. `e2e3eaba53bef1b6a7ddb6648b03b0d2`.
- **`time_segment`**: The start and end times (in seconds) for the track, i.e. `[9.43, 14.60]`.
- **`track_num`**: The order number of the track, typically assigned based on the start time, i.e. `10`.
- **`locations`**: An object containing the 3D location, 2D bounding box, and fixture assignment for various frames.
  - **`start`**: The object's location, bbox and fixture at the start of the track. `Null` if no start frame.
    - **`frame`**: The starting frame number, i.e. `28`.
    - **`3d_location`**: A three-element list representing the world-coordinates X, Y, and Z coordinates in 3D space, i.e. `[-0.1, -3.1, -0.02]` and `Null` if not 3D location avaiable.
    - **`bbox`**: A four-element list specifying the 2D bounding box `[xmin, ymin, xmax, ymax]`, i.e. `[693.1, 847.2, 775.00, 979.8]`.
    - **`fixture`**: A string indicating the fixture the object is assigned to, i.e. `P01_cupboard.009` and `Null` if no assigned fixture. 
    For more information about the scene fixtures, please see [Digital Twin Scene Documentation](https://hd-epic.github.io/#digital-twin-scene--object-movements).
  - **`end`**: The object's location, bbox and fixture at the end of the track, with the same structure as `start`. 
  - **`other_frames`**: A list of other annotated frames for the track with the same structure as `start`. 

Moreover, the long-term associations between the tracks are coming soon, please keep an eye on our website: [HD-EPIC](https://hd-epic.github.io/#digital-twin-scene--object-movements). 

## High Level

This contains the high level activities as well as recipe and nutrition information

### activities / PXX_recipe_timestamps.csv

**Field Descriptions**:
- **`video_id`**: A unique identifier for the video ID, i.e. `P01-20240202-110250`.
- **`recipe_id`**: If the activity is part of the recipe, the recipe ID (for this participant) is noted. Leave empty for background activities.
- **`high_level_activity_label`**: General description of high level activity.

### complete_recipes.json

**Field Descriptions**:
- A unique identifier for each recipe formed of PXX-RYY, where XX is the participant id and YY is the recipe ID, unique for that participant
- **`participant`**: Participant ID.
- **`name`**: Name of that recipe.
- **`type`**: Indicates whether the recipe is available as is online, or has been modified/adapted from an online or written source
- **`source`**: A link to the online recipe before adaptation. Note that these links might no longer be available if the recipe is taken down from source.
- **`steps`**: The ordered free form steps (as done by the participant, so could be modified from the source). Each step has a unique step ID
- **`captures`**: If the recipe is done multiple times, then each is considered a separate capture. This is the case for a few recipes like coffee and cereal breakfast.
   - **`videos`**: These are the one or more videos that contain the steps of this recipe
   - **`ingredients`**: The list of ingredients and their nutrition. Note that the nutrition might differ across captures.
      - A unique ingredient ID
      - **`name`**: name of the ingredient in free form
      - **`amount`**: If known, the amount of the ingredient added to the recipe.
      - **`amount_unit`**: whether the measurement is in units, grams, ml, ...
      - **`calories`**: the amount of calories of this ingredient in the amount specified.
      - **`carbs`**: carbs
      - **`fat`**: fat
      - **`protein`**: protein
      - **`weigh`**: the segments in the videos of when this ingredient is weighed - whether on the digital scale or through another measurement (e.g. spoon)
      - **`add`**: the segments in the videos when this ingredient is added to the recipe.

## Audio annotations

This folder contains audio annotations HD_EPIC_Sounds (in csv and pkl) structured as follows: 

### `HD-EPIC-Sounds.csv`

This CSV file contains the sound annotations for HD-EPIC and contains 9 columns:

| Column Name           | Type                       | Example                     | Description                                                                   |
| --------------------- | -------------------------- | --------------------------- | ----------------------------------------------------------------------------------- |
| `participant_id`      | string                     | `P01`                       | ID of the participant (unique per participant).                                     |
| `video_id`            | string                     | `P01-20240202-110250`       | ID of the video where the segment originated from (unique per video).               |
| `start_timestamp`     | string                     | `00:00:00.476`              | Start time in `HH:mm:ss.SSS` of the audio annotation.                               |
| `stop_timestamp`      | string                     | `00:00:02.520`              | End time in `HH:mm:ss.SSS` of the audio annotation.                                 |
| `start_sample`        | int                        | `22848`                     | Index of the start audio sample (48KHz) in the untrimmed audio of `video_id`.  |
| `stop_sample`         | int                        | `120960`                    | Index of the stop audio sample (48KHz) in the untrimmed audio of `video_id`.        |
| `class`               | string                     | `rustle`                    | Assigned class name.                                                                |
| `class_id`            | int                        | `4`                         | Numeric ID of the class.                                                      |

## VQA-benchmark

These JSON files contain all the questions for our benchmark, with each file containing the questions for one question prototype

**Field Descriptions**:
- **`inputs`**: The visual input for the question and any bounding boxes. This could be one or more videos, one or more clips and optionally one bounding box.
- **`question`**: The question in the VQA
- **`choices`**: The 5-option choices
- **`correct_idx`**: The index (start from 0) of the correct answer.

## Youtube Links

This contains the links to all videos of the dataset. Notice that YouTube introduces artifacts to the videos, so these should only be used for viewing the videos. Please download the videos themselves from our ![webpage](https://hd-epic.github.io/index#download) in the full quality to do any processing or replicate the VQA results

### `HD_EPIC_VQA_Interface.html`

An interface to visualise all our VQA questions

**Contact:** [uob-epic-kitchens@bristol.ac.uk](mailto:uob-epic-kitchens@bristol.ac.uk)
