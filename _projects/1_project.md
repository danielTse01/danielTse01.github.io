---
layout: page
title: Dota 2 Draft Picker
description: NC Hackathon 2022
img: assets/img/dota2_background.jpg
importance: 1
category: my work
related_publications: true
---

Dota 2 is an online team-based game where two teams of five players each control unique characters called "heroes" with special abilities. The objective is to destroy the enemy team's main structure, the "Ancient," located in their base, while defending their own. The game is played on a large map with three main paths, or "lanes," where computer-controlled units, called "creeps," regularly spawn and move towards the enemy base. Success requires teamwork, strategy, and quick reflexes, as players must balance attacking, defending, and strengthening their heroes throughout the match.

I chose to do a project on Dota 2 since it is one of my favorite games of all time. No two Dota games will be alike, and the strategies players can use against each other creates a beautiful, yet extremely competitive and fun atmosphere for the most dedicated strategy player.

The inspiration for this project is simply that I was losing games. Other than my brilliant idea of using hacks or cheating to win, I thought the next best step was to create a program that helped me strategize my playstyle to victory. My focus on this project was to create a program that helped me pick the best "hero", or character, that would give me the best chances of winning a game.

In Dota 2, drafting is the process where teams take turns selecting their heroes before the match begins. Each team strategically picks heroes to create a balanced and synergistic lineup while also countering the enemy team's choices. Drafting is crucial because the combination of heroes can significantly impact the game's outcome; a well-drafted team can exploit the enemy's weaknesses and maximize their own strengths.


<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
