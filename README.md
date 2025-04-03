## ![logo](logo-white.png) HD-EPIC: A Highly-Detailed Egocentric Video Dataset (CVPR 2025)


<!-- start badges -->
[![arXiv-2502.04144](https://img.shields.io/badge/arXiv-2502.04144-green.svg)](https://arxiv.org/abs/2502.04144)
<!-- end badges -->

## Project Webpage
Dataset - download and further information is available from [Project Webpage](https://hd-epic.github.io/)

Paper is available at [ArXiv](https://hd-epic.github.io/)

## Citing
When using the dataset, kindly reference:
```
@InProceedings{perrett2025hdepic,
  author    = {Perrett, Toby and Darkhalil, Ahmad and Sinha, Saptarshi and Emara, Omar and Pollard, Sam and Parida, Kranti and Liu, Kaiting and Gatti, Prajwal and Bansal, Siddhant and Flanagan, Kevin and Chalk, Jacob and Zhu, Zhifan and Guerrier, Rhodri and Abdelazim, Fahd and Zhu, Bin and Moltisanti, Davide and Wray, Michael and Doughty, Hazel and Damen, Dima},
  title     = {HD-EPIC: A Highly-Detailed Egocentric Video Dataset},
  booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)},
  year      = {2025},
  month     = {June}
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
We annotate object movements by labeling temporal segments from pick-up to placement and 2D bounding boxes at movement onset and end. Tracks include even slight shifts/pushes, ensuring full coverage of movements. Every object movement is annotated and assgin to a scene fixture, providing a rich dataset for analysis. Movements of the same object are then grouped into "associations" by human annotators. This association data is stored across two JSON files. The first (`scene-and-object-movements/assoc_info.json`) is a JSON object where the keys are video names and the values are groupings of each object's movements throughout the video (referred to as "associations"). The structure for this file is as follows:
```jsonc
{
  "video_id": {
    "association_id": {
      "name": "string",
      "tracks": [
        {
          "track_id": "string",
          "time_segment": [start_time, end_time],
          "masks": ["string", ...]
        },
        ...
      ]
    },
    ...
  }
}
```
The string IDs in "masks" can then be used to query the second JSON file (`scene-and-object-movements/mask_info.json`) for information on MP4 frame number, 3D location, bounding box and scene fixture of each object mask. The structure of this JSON object is as follows:
```jsonc
{
  "video_id": {
    "mask_id": {
      "frame_number": integer,
      "3d_location": [x, y, z],
      "bbox": [xmin, ymin, xmax, ymax],
      "fixture": "string"
    },
    ...
  }
}
```
Each `mask_id` can be matched to a mask file name (e.g. `frame_id.png`) in the [dropbox](https://www.dropbox.com/scl/fo/f7hwei2m8y3ihlhp669h4/ALM8_1LDETY40O-06-ptr3A?rlkey=yrmqm3zk284htr5yjxb4z5nwp&e=1&st=815ovw6m&dl=0). It should be noted that the masks and bounding boxes were completed by different teams and therefore may be inconsistent in places.

**Field Descriptions**
- **`video_id`**: The name of the video, i.e. `P01-20240202-110250`
- **`association_id`**: A unique identifier for the object movement tracks
- **`name`**: The name of the association, i.e. `plate`
- **`tracks`**: A list of object movements that make up the association
- **`track_id`**: A unique identifier for the single movement of the object in the association
- **`time_segment`**: A start and end time for the single movement of the object in the association
- **`masks`**: A list of unique identifiers for each object mask connected to this particular movement of the object
- **`mask_id`**: A unique identifier for the object mask. This can be matched to a mask ID in the `masks` field of `assoc_info.json`, if this frame is connected to an association
- **`frame_number`**: The MP4 frame number for the particular frame
- **`bbox`**: A four-element list specifying the 2D bounding box `[xmin, ymin, xmax, ymax]`, i.e. `[693.1, 847.2, 775.00, 979.8]`.
- **`fixture`**: A string indicating the fixture the object is assigned to, i.e. `P01_cupboard.009` and `Null` if no assigned fixture.

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

This contains the links to all videos of the dataset. Notice that YouTube introduces artifacts to the videos, so these should only be used for viewing the videos. Please download the videos themselves from our [webpage](https://hd-epic.github.io/index#download) in the full quality to do any processing or replicate the VQA results

### `HD_EPIC_VQA_Interface.html`

An interface to visualise all our VQA questions

**Contact:** [uob-epic-kitchens@bristol.ac.uk](mailto:uob-epic-kitchens@bristol.ac.uk)
