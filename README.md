# Statcast xWAR
Using Baseball Savant statistics to formulate a wins above replacement statistic.

## What's in here?
- xwar_obj.ipynb

Jupyter notebook that uses .csv files from Fangraphs and Statcast to determine xWAR for all players in a given year. The `calculate_xwar` function recieves the parameters for a given year, and a boolean as to whether or not the Fangraphs base-running value should be incorporated into xWAR as-is. *xwar_obj* also includes some code that analyzes the predictiveness and correlation between xWAR and fWAR, as well as some plotting.

- index.html

HTML code from the Bokeh plot generated by `xwar_obj.ipynb`. The interactive itself can be found at https://dev-1999.github.io/statcast_xwar/

- data
  - 20XX_catcher_framing.csv
  Statcast catcher framing data from Stacast from 2017-2020. Relevant column is `runs_extra_strikes`.
  - 20XX_expected_stats.csv
  Statcast xwOBA and expected batting stats from 2017-2020. `est_woba` or xwOBA is the relevant column.
  - 20XX_fg_value.csv
  Fangraphs' fWAR csv from 2017-2020 broken down by component. `Positional` and `Base Running` are used to calculate xWAR, fWAR is used as baseline elsewhere.
  - 20XX_outs_above_average.csv
  Statcast fielding data, `fielding_runs_prevented` is relevant column used to determine defensive contributions of non-catchers.
  - guts.csv
  Guts page from Fangraphs, used to incorporate wOBA Scale, Runs Per Win, and added additional columns for games per year and average xwOBA for seasons in OAA era.
  - playeridmap.csv
  Downloaded csv that maps player names, MLB IDs, and Fangraphs IDs.
  
- outputs

 Created xWAR leaderboards (with components) for years 2017-2020.
 
- images

 Photos for the writeup below.
 
 ## Full Writeup
 Using Statcast Stats to Create a Wins Above Replacement Metric

Baseball Savant has overtaken Fangraphs as my primary resource for pulling up baseball player pages. I think a big part of this is the narrative that the data (and viz) tells right away. You know exactly what this guy is good at (most things):

![judge](https://raw.githubusercontent.com/dev-1999/statcast_xwar/main/images/judge.png)

And what this guy is good at (the other things):

![fletcher](https://raw.githubusercontent.com/dev-1999/statcast_xwar/main/images/fletcher.png)

Part of the enjoyment of a site like Baseball Savant is piecing together how good people are from this information, but it'd also be nice to synthesize all this information into a single, sortable statistic. This is why I created xWAR.

The main components of WAR for position players are runs from batting, fielding, baserunning, and positional adjustment. I took the latter two figures directly from Fangraphs for these calculations, but I substituted Statcast's batting and fielding stats here instead. Instead of using wOBA as the basis for calculating wRAA (and subsequently batting runs), xWAR uses xwOBA. Where Fangraphs uses UZR in their calculations, xWAR uses Statcast's Runs Extra Strikes for catchers and Fielding Runs Prevented, based on Outs Above Average but with some extra weighting to plays that save extra base hits, for everyone else. (It's not a big deal for discerning between corner/middle infielders but is very different when comparing IF and OF: http://tangotiger.com/index.php/site/comments/statcast-lab-is-there-a-different-run-value-needed-based-on-the-infield-slice)

This points to the first scenario in which xWAR is a bit of a misnomer for this calculation - it combines things that aren't outcome dependent on an individual basis (xwOBA) with fielding stats that reflect the outcome of a play. Because of this, xWAR isn't so much of an answer to the question "how good was this player completely outside the influence of his circumstances", but more so an answer to the question "how good does Statcast suggest this player is" - which is more in line with what motivated this project anyway.

This also suggests that xWAR is no more predictive than regular WAR - not very. xWAR can only be calculated from the 2017 season onwards (Statcast's OAA history), and the level of correlation between xWAR and the following year's fWAR is low, similar to the correlation between fWAR and fWAR from the following year. The r2 score between xWAR and fWAR from the same season is strong however, as can be seen in the table below.

Year	R2

2017	0.816

2018	0.844

2019	0.846

2020	0.654

Plus, xWAR and fWAR follow similar distributions:

![histogram](https://raw.githubusercontent.com/dev-1999/statcast_xwar/main/images/hist.png)

Another limitation of this project is that the inputs for defense are whole numbers. Presumably a decimal-accurate measure of runs saved from framing/defense exists somewhere in the Google CloudTM but xWAR incorporates only the outward facing stuff from Baseball Savant. Furthermore, fWAR (and others) account for league adjustment and park factors, and I omitted both from my calculation of xWAR. I did use Fangraphs' calculations for the wOBA Scale and the Runs/Win value for each year for simplicity's sake - you can read how Fangraphs' calculates WAR for position players here: https://library.fangraphs.com/war/war-position-players/.

Skip to this part if you want to see the leaderboards - you can also get the full output and code on Github here:

And a direct link to interactive plots of the data: https://dev-1999.github.io/statcast_xwar/

Best Seasons By xWAR


Worst Seasons by xWAR


Biggest xWAR Underperformers


Dansby Swanson's 2019 offers the most compelling case of a high xWAR being a harbinger of positive regression to come. Fangraphs and Savant switched opinions on Swanson's defense in '19 and '20. As you'd imagine, Savant was more bullish with a 62nd percentile OAA in 2019 but a 11th percentile grade in 2020. (Small sample size though). Fangraphs credited Swanson with 3.6 extra runs on defense in 2020 as compared to 2019, however (SSS). Furthermore, by bringing his 2020 wOBA up towards the level of his 2019 and 2020 xwOBA, 2020 Swanson put up 1.9 fWAR in 264 PA.

2017 Nick Castellanos hit the ball super hard without much to show for it. Fangraphs docked Nick 15 runs from defense but Statcast was more generous by subtracting "only" 11. 2018 Castellanos posted 3.0 fWAR over a full season, absolutely no thanks to any contributions in the field by either Fangraphs or Statcast's judgement. 2018 Castellanos was the worst defender in baseball by 6 runs, posting a bilious -22 Runs Prevented and -24 OAA.

Biggest xWAR Overperformers


Other names on these lists are, for the most part, due to big disagreements between OAA and UZR and/or a hitter substantially underperforming or outperforming their xwOBA over the course of a full season, or a combination of both (ex: 2019 Lorenzo Cain, 2018 Rougned Odor). As such, I'm not sure how much I prefer using only a player's xWAR to describe their season when the granular data is so readily available and clearly presented, but xWAR is a valid synthesis of Statcast data into one number nonetheless.
