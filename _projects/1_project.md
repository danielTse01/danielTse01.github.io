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
        {% include figure.liquid loading="eager" path="assets/img/dota2_draftingscreen.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This is what the screen looks like for a player during the drafting phase. All the icons in the middle represent a unique playable "hero" a person can choose to play.
</div>

My program relies on webscraping the information provided by [DotaBuff](https://www.dotabuff.com/) on the winrates of every hero, and as well as using the official Dota 2 API called [OpenDota](https://docs.opendota.com/).

The Dota 2 Draft Picker program calculates summation of the winrate advantage of every single "hero" against an inputted list of "heros".


For example, let's say the opposing team has drafted the two heroes [Medusa](https://www.dota2.com/hero/medusa) and [Templar Assasin](https://www.dota2.com/hero/templarassassin) so far, and we pick a "hero" that can is very strong against these two. Below is are images from DotaBuff that shows best counters to these two heroes individually.


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dota2_medusa.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/dota2_templar.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Dotabuff has all the "Disadvantage" percentages calculated for every hero in the game when viewing a specified hero.
</div>


{% endraw %}