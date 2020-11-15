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
 
- Images
 Photos for the writeup below.
 
 ## Full Writeup
 Using Statcast Stats to Create a Wins Above Replacement Metric

Baseball Savant has overtaken Fangraphs as my primary resource for pulling up baseball player pages. I think a big part of this is the narrative that the data (and viz) tells right away. You know exactly what this guy is good at (most things):


And what this guy is good at (the other things):


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



Another limitation of this project is that the inputs for defense are whole numbers. Presumably a decimal-accurate measure of runs saved from framing/defense exists somewhere in the Google CloudTM but xWAR incorporates only the outward facing stuff from Baseball Savant. Furthermore, fWAR (and others) account for league adjustment and park factors, and I omitted both from my calculation of xWAR. I did use Fangraphs' calculations for the wOBA Scale and the Runs/Win value for each year for simplicity's sake - you can read how Fangraphs' calculates WAR for position players here: https://library.fangraphs.com/war/war-position-players/.

Skip to this part if you want to see the leaderboards - you can also get the full output and code on Github here:

And a direct link to interactive plots of the data: https://dev-1999.github.io/statcast_xwar/

Best Seasons By xWAR
	  full_name	        pa	year	fWAR	xWAR
1	  Mookie Betts	    614	2018	10.4	9.32
2	  Mike Trout      	600	2019	8.6	9.18
3	  Aaron Judge	      678	2017	8.3	9.12
4	  Mike Trout	      608	2018	9.8	8.90
5	  Cody Bellinger	  661	2019	7.8	8.43
6	  Christian Yelich	651	2018	7.6	8.06
7	  Mookie Betts	    706	2019	6.6	7.95
8	  Francisco Lindor	745	2018	7.6	7.94
9	  Anthony Rendon	  646	2019	7.0	7.57
10	Ronald Acuna Jr.	715	2019	5.6	6.83
11	Juan Soto       	659	2019	4.8	6.71
12	Jose Ramirez    	698	2018	8.0	6.69
13	Christian Yelich	580	2019	7.8	6.66
14	Anthony Rendon	  597	2018	6.2	6.64
15	Manny Machado	    709	2018	6.2	6.61
16	Francisco Lindor	723	2017	5.7	6.60
17	Corey Seager	    613	2017	6.0	6.55
18	Alex Bregman	    705	2018	7.6	6.45
19	Lorenzo Cain	    620	2018	5.7	6.34
20	Justin Turner	    543	2017	5.4	6.28
21	Mike Trout	      507	2017	6.8	6.22
22	Paul Goldschmidt	665	2017	5.3	6.16
23	Joey Votto	      707	2017	6.5	6.15
24	Josh Donaldson  	659	2019	4.9	6.10

Worst Seasons by xWAR
	  full_name	       pa	year	fWAR	xWAR
1	Jeff Mathis	      244	2019	-2.1	-1.62
2	John Hicks	      333	2019	-1.2	-1.56
3	Mark Trumbo	      603	2017	-1.3	-1.53
4	Chris Davis	      522	2018	-3.2	-1.30
5	Carlos Beltran	  509	2017	-1.1	-1.28
6	Adam Rosales	    312	2017	-0.3	-1.27
7	Meibrys Viloria	  148	2019	-0.9	-1.27
8	Darwin Barney	    362	2017	-0.5	-1.24
9	Stevie Wilkerson	361	2019	-0.6	-1.16
10	Dwight Smith	  392	2019	-0.8	-1.16
11	Alexi Amarista	176	2017	-1.1	-1.14
12	Jo Adell	      132	2020	-1.3	-1.13
13	Daniel Palka	  449	2018 	 0.7	-1.12

Biggest xWAR Underperformers
	full_name	            pa	year	fWAR	xWAR	Difference
1	  Dansby Swanson	    545	2019	1.5	  4.15	-2.65
2	  Lorenzo Cain	      623	2019	1.5	  4.09	-2.59
3	  Rougned Odor	      651	2017	-1.4	1.16	-2.56
4	  Nick Castellanos	  665	2017	1.5	  3.89	-2.39
5	  Miguel Cabrera	    529	2017	-0.2	2.18	-2.38
6	  Mitch Moreland	    576	2017	0.9	  3.21	-2.31
7	  Jackie Bradley Jr.	535	2018	2.8	  4.94	-2.14
8	  Kole Calhoun	      552	2018	0.0	  2.14	-2.14
9	  Justin Smoak	      500	2019	0.2	  2.34	-2.14
10	Khris Davis	        652	2017	2.5	  4.63	-2.13
11	DJ LeMahieu	        682	2017	2.0	  4.11	-2.11
12	Manny Machado	      690	2017	2.4	  4.48	-2.08
13	Kendrys Morales	    471	2018	0.2	  2.23	-2.03
14	Rougned Odor	      581	2019	0.3	  2.30	-2.00

Dansby Swanson's 2019 offers the most compelling case of a high xWAR being a harbinger of positive regression to come. Fangraphs and Savant switched opinions on Swanson's defense in '19 and '20. As you'd imagine, Savant was more bullish with a 62nd percentile OAA in 2019 but a 11th percentile grade in 2020. (Small sample size though). Fangraphs credited Swanson with 3.6 extra runs on defense in 2020 as compared to 2019, however (SSS). Furthermore, by bringing his 2020 wOBA up towards the level of his 2019 and 2020 xwOBA, 2020 Swanson put up 1.9 fWAR in 264 PA.

2017 Nick Castellanos hit the ball super hard without much to show for it. Fangraphs docked Nick 15 runs from defense but Statcast was more generous by subtracting "only" 11. 2018 Castellanos posted 3.0 fWAR over a full season, absolutely no thanks to any contributions in the field by either Fangraphs or Statcast's judgement. 2018 Castellanos was the worst defender in baseball by 6 runs, posting a bilious -22 Runs Prevented and -24 OAA.

Biggest xWAR Overperformers
	  full_name	           pa	year_x	WAR	xWAR	Difference
1	  Alex Bregman	      690	2019	  8.5	5.31	3.19
2	  Didi Gregorius	    570	2017	  4.1	1.10	3.00
3	  Scooter Gennett	    638	2018	  4.5	1.64	2.86
4	  Yasmani Grandal	    482	2017	  4.1	1.29	2.81
5	  Zack Cozart	        507	2017	  5.0	2.21	2.79
6	  Eduardo Nunez	      491	2017	  2.2	-0.48	2.68
7	  Marwin Gonzalez	    515	2017	  4.0	1.36	2.64
8	  Dee Strange-Gordon	695	2017	  3.6	0.97	2.63
9	  Matt Chapman	      616	2018	  6.6	4.25	2.35
10	Eddie Rosario	      592	2018	  3.5	1.22	2.28
11	Didi Gregorius	    569	2018	  4.7	2.46	2.24
12	Brett Gardner	      550	2019	  3.6	1.40	2.20
13	Yuli Gurriel	      612	2019	  3.2	1.01	2.19
14	Corey Dickerson	    629	2017	  2.6	0.49	2.11
15	Jose Altuve	        662	2017	  7.6	5.49	2.11
16	Marcus Semien	      747	2019	  7.6	5.51	2.09
17	Xander Bogaerts	    698	2019	  6.8	4.72	2.08
18	Fernando Tatis Jr.	372	2019	  3.6	1.54	2.06
19	Ketel Marte	        628	2019	  7.1	5.06	2.04

Other names on these lists are, for the most part, due to big disagreements between OAA and UZR and/or a hitter substantially underperforming or outperforming their xwOBA over the course of a full season, or a combination of both (ex: 2019 Lorenzo Cain, 2018 Rougned Odor). As such, I'm not sure how much I prefer using only a player's xWAR to describe their season when the granular data is so readily available and clearly presented, but xWAR is a valid synthesis of Statcast data into one number nonetheless.
