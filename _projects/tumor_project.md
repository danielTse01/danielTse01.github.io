---
layout: page
title: TensorFlow Brain Tumor Image Classification
description: Personal
img: assets/img/early-detection-of-brain-tumors-and-beyond-354432-960x540.jpg
importance: 1
category: my work
---

## Introduction
Brain tumors pose significant challenges in medical diagnostics, necessitating accurate and efficient identification methods. This project aims to develop a robust model capable of classifying three brain tumor types from MRI images: pituitary, glioma, and meningioma tumors. By utilizing advanced neural networks and comprehensive data augmentation techniques, this project strives to enhance diagnostic accuracy and provide valuable support to medical professionals in early detection and treatment planning of brain tumors.

## Dataset
The images that make up this dataset comes from [Brain Tumor Classification (MRI)](https://www.kaggle.com/datasets/sartajbhuvaji/brain-tumor-classification-mri). The dataset contains 4 separate classification materials for brain tumors, totaling in 3160 brain scans. The 4 classifications include pituitary, glioma, meningioma and no tumors. Below are examples of each type of tumor, and the cross sections of scans exist in all three planes of the brain.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/tum1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is what the screen looks like for a player during the drafting phase. All the icons in the middle represent a unique playable "hero" a person can choose to play.
</div>

My program relies on webscraping the information provided by [DotaBuff](https://www.dotabuff.com/) on the winrates of every hero, and as well as using the official Dota 2 API called [OpenDota](https://docs.opendota.com/).

The Dota 2 Draft Picker program calculates summation of the winrate advantage of every single "hero" against an inputted list of "heros".

For example, let's say the opposing team has drafted the two heroes [Medusa](https://www.dota2.com/hero/medusa) and [Templar Assasin](https://www.dota2.com/hero/templarassassin) so far, and we pick a "hero" that can is very strong against these two. Below is are images from DotaBuff that shows best counters to these two heroes individually.

<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dota2_medusa.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dota2_templar.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Dotabuff has all the "Disadvantage" percentages calculated for every hero in the game when viewing a specified hero.
</div>

In the images above, I have highlighted a hero named [Dark Seer](https://www.dota2.com/hero/darkseer), that seems to counter both Medusa and Templar Assassin. The way to read the "Disadvantage" percent to subtract 50% - (disadvantage percent). For exampple with Medusa, the disadvantage percent is 7.30%, meaning that Medusa has a 42.7% chance to win against dark seer in a Dota game. Average winrate in Dota should be exactly 50%, meaning 42.7% is poor chance that Medusa will win this hypothetical game.

Going back to what was said previously, if the opposing team has drafted Medusa and Templar Assassin, it seems Dark Seer is a very strong pick against these two. By adding the disadvantage percents of Dark Seer against Medusa and Templar Assassin, we get a score of 7.3 + 4.99 = 12.29. The Dota 2 Draft Picker does this calculation for every hero in the game against these two, and the output will show the highest scores.

This program is able to counter opposing drafts at all stages of the picking phase, and it even contains a feature that will draft an entire team against an opposing draft.

To run and test this program, click on this Replit Link to take you to the [Dota 2 Draft Picker](https://replit.com/@schquid98/Dota-2-Draft-Picker).
