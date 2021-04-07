---
title: "Visualize expected goals"
date: 2021-04-07T22:38:00+10:00
slug: "visualize-xg"
description: "A visualization of expected goals in soccer."
draft: false
tags: ["Soccer", "Visualization", "xG"]
math: true
showToc: true
---

When talking about the result of a soccer game, we are actually talking about two scores: the numbers of goals scored by home and away respectively. But sometimes, if you are a soccer fan, you probably would tell me Christian Pulisic scored his first goal since December in the 27th minute into Chelsea's 2:5 lost to West Brom. So that's two more variants: *time* and *player*. When it comes to *expected goals*, we could probably consider something like *how the score ended up like this*, or *who really caused lots of trouble for Chelsea*.

To answer these questions in a more direct approach, we can visualize the expected goals (xG), from three internally related but distinct perspectives.

I find it sometimes difficult to crawl advanced data from understat by hand. Therefore, I use `{understatr}` package to retrieve the data we need. The `get_match_shots(id)` function solves 99% of the problem here. The `id` is part of the url of a specific match. Still `{understatr}` is not that omnipotent enough to bring out everything we can see on understat.com such as the ones hidden in tabs.

## xG timeline

The plot itself is nothing but a regular graph drawn by `geom_step()` function. The tricky part so far is to add both starting and ending records to the data frame since we are not expecting any dramatic events when t=0 or t=90. Additionally, most of the game does not end when t=90, so it might be a good idea to find the last non-goal event in game and set it as the other limit of x-axis.

But here comes a question:

> What if we have a goal in the 45+2 th minute in the first half, and the very 1st minute of the second half? How are we gonna draw the timeline with data like that?

Then I use `geom_point()` to mark the actual goals/own goals events, and `ggforce::geom_mark_circle()` to annotate the point with extra labels. I intentionally write a loop for this step of "adding goals one by one" because I don't want to repeat myself for different matches.

Being lazy is always a main drive of maintaining reproducibility. If it saves my time, it is a good one.

<img class="special-img-class" style="auto" src="/image/visualize-xg/18720-Chelsea-vs-West%20Bromwich%20Albion-xg-timeline.png" />

As you can see, there are some obvious issues in the graph:

1. When multiple goals occur in a rather short time period, even though I have set a `label.buffer` of 5mm to prevent labels covering data points, the labels themselves are overlapping each other.
2. I only pick the color palette from both sides' team kit and naïvely apply them to a solarized ggplot theme. Not a good pair of contrasting colors in this case.

## Shots graph

The next step of playing around with xG data is to find out where those shots, and specifically those goals, happened?

The shots data from previous visualization still prove to be useful in this one. Without any documentation, I have to do the trial and error to see what those `X` and `Y` coordinates mean. It turns out that those are not actually coordinates of a standard pitch size, but the percentage of a pitch. So `X = .5, Y = .5` means the shot took place at the center spot, which is almost impossible.

However, the main function `ggsoccer::annotate_pitch` to draw soccer event data on pitch has an attribute `dimentions` which takes different formats of X-Y coordinates. For example,

- `pitch_opta` accepts `X` and `Y` ranging from 0 to 100.
- `pitch_statsbomb` accepts `X` from 0 to 120, `Y` from 0 to 80, etc.

Here we just map `[0, 1]` to `[0, 100]` with `pitch_opta`.

Another note for the size of shot points: although the size of a point should better be proportional to its value, I still make a decision to transform the value of xG to make the dots more clear in this one.

<img class="special-img-class" style="auto" src="/image/visualize-xg/18720-Chelsea-vs-West%20Bromwich%20Albion-xg-court.png" />

After taking a detour to `{StatsBombR}` and other function in `{ggsoccer}`, I manage to finish the part 2 with more questions in my head:

> Any better approach to represent distinguished shot results? Which side should I draw an own goal on?

I really don't like the idea of introducing a set of colors or symbols to complicate the situation.

## Individual xG

The last part of this visualization project is to gain an insight of individual player's xG, in comparison with other players' of course. A treemap with a subgroup of teams will fit this scenario.

<img class="special-img-class" style="auto" src="/image/visualize-xg/18720-Chelsea-vs-West%20Bromwich%20Albion-xg-contr.png" />

## What's next?

All the three graphs can be automatically drawn by inputting a match id, a home team kit color and an away team kit color. But searching and typing hex values is straight painful. I have been wondering if there is a decent way to acquire these through API while keeping them distinguishable enough. The closest answer to this is a website called [Team Color Codes](https://teamcolorcodes.com/), which collects and organized the team palette of major sports teams pretty tidy. The pity is you still have to pull the trigger on your own.

In addition, the whole idea of doing this is to give an audience some rough idea about what xG is. But questions remain:

> How is xG calculated? And why is it done in this way?

There always exists a gap between "expectation" and "reality"? Ideally, a good model of xG should shrink the gap by all means.

> So, how do we evaluate an xG model?

***

This project is inspired by Ryo Nakagawara's blog post [Visualizing the Premier League So Far, Part 1: Overview with xPts Tables and xG Plots](https://ryo-n7.github.io/2019-11-21-visualize-EPL-part-1/).

You can find the script used above in my [repository](https://github.com/rexarski/box2box/blob/main/match-analysis/xg-and-stats.R) to generate similar graphs.
