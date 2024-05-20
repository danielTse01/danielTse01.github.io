---
layout: page
title: Dota 2 Draft Picker
description: NC Hackathon 2022
img: assets/img/dota2_background.jpg
importance: 3
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
