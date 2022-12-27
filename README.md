# The Data Science of Pokémon
*A Guide to Data Science... and Pokemon*

## What is this?
The first Generation of the Pokémon main game (released in 
1996/98/99 (JP/US/EU)) was a milestone in gaming
and is still popular: the speed-run world record is from 2020. 
This first generation of Pokémon is also captured in some kaggle 
data sets and notebooks. However, while these insights are surely 
interesting, they miss certain points which highlight common general
shortcomings in today´s popular machine learning culture. I´d like to
discuss these issues in this project and propose criteria to achieve
meaningful insights with machine learning and data in general.

<img src="fights_and_advantages.png" width="800">

https://public.tableau.com/app/profile/mankle26/viz/Pokemon_Starters_Gen_1/FightsAdvantages 


### Context matters!
First: Ego-Perspective

Many analyses are structured around the list of Pokémon and their
"stats" (Attack, Defense...). Many conclude that Mewtu is the 
strongest Pokémon because it has the best stats in that regard.
However, such-like analyses completely ignore the gameplay, which
allows you to conquer the world of Pokémon gradually mostly through 
success in Pokémon fights. Why is this important?

Well, if one starts the game and wants to know which Pokémon to use,
the advice "Mewtu is the best." does not help much, because it is 
literally the last Pokémon you can catch in the game.The solution I 
suggest is to look at those Pokémon which can be owned 
right from the beginning and compare only those. 

Second: Alter-Perspective

Your own Pokémon is still only half of the story. What also matters
is which Pokémon you will have to defeat during the game. What does
it help if you have one that is super-effective in fights against
Ice-Type if those make only 2% of all fights (which is actually the case...)?

For this analysis, I compiled a data set which includes information
on every single fight you have to win within the game to evaluate
which strategy could be beneficial for the game as a whole.


### Applicability matters!
There are different features to evaluate the power of a Pokémon. 
Two frequently used are their stats (Attack, Defense...) and 
sometimes their type. While its type always matters to determine 
the dis-/advantages of a Pokémon in defense, it is also often assumed
that this also matters in determining its offensive dis-/advantages.

However, this is not the case. Many Pokémon can not learn attacks of
their type. Of the twelve Bug-Type Pokémon, only five can learn 
attacks of this type, while three Pokémon which are not of type Bug
can. This already shows that reality might be more complicated than a 
handy data set might suggest. 

## Conclusion
Getting data and analyzing it is not difficult. The real challenge is 
to look beyond the seemingly smooth and ready-to-use data, 
understand its context and 
analyze it with a focus that allows for real world applications and
not purely theoretical - and sometimes trivial - conclusions.
This could also mean that one concludes that present data is not 
useful in allowing such insights and that it might be necessary to 
collect different or extended data. It might be a hassle but just 
ignoring the shortcomings is no solution. 

In this project, I tried to illustrate these points using the example
of Pokémon. All files I used can be found in this repository. The 
Visualization was generated using, and can be found on, Tableau Public:
https://public.tableau.com/app/profile/mankle26/viz/Pokemon_Starters_Gen_1/FightsAdvantages

__________

## _The fine print_

This part documents the steps and decisions taken by the researcher
to arrive at the above data, visualizations and conclusions:

- The game data:
The main data set "pok_fights" was compiled using the most recent 
speedrun of Pokémon Gen 1: https://www.youtube.com/watch?v=ZNG3tkrxVdQ
Every Pokémon fight received one entry/row, containing information
on the opponent Pokémon including name, level, types and moves. These
were collected from www.pokewiki.de and translated to english using
https://bulbapedia.bulbagarden.net/wiki/List_of_moves_in_other_languages.
The frequency of each type among these opponents is displayed in the
above bar chart and compared to the frequency of each type among all
Pokémon of the first generation.
I then checked for every move whether it deals type-specific damage (
many moves do no damage at all and others deal damage but not of their
type, e.g. Dragon Rage) and entered this type in a new variable.


- The Starters data:
The above data was then extended for data on the three official starters
and Nidoran♂, which is a Pokémon you can catch after the first fight in 
the game and is often used in speedruns instead of one of the starters.
To keep the data comparable, I decided to drop the first row, the fight
against the game-intern Rival. In this first fight right after picking
a starter, there are no differences between the different starters (
all of them have only normal moves) and this decision therefore did 
not disadvantage one of them. I then checked for each of these four
Pokémon which Types they have. In the cases of Charmander and Nidoran♂
their types change when they evolve during the game, which I included
at the appropriate row. I then computed for every row whether there are
type-dis-/advantages between the opponent´s type-specific damage dealing
moves and the types of the starters (here only looking at the starters
in a defensive role). This generated a score between 0 and 2 for each
attack-type combination. When taking into account the second type of the
starter these values can even range between 0 and 4 (dis-/advantages
are multiplied). I then took the maximum of these up to four values (
each Pokémon has up to 4 moves at a time) and stored it. I then
conducted the same procedure the other way round: What moves can the 
starter have at which point of the game and how do those affect the 
respective opponent? The resulting value was also stored in a new
variable. Finally, I took the difference of these values, subtracting
the maximum disadvantage in defense from the maximum advantage in 
offense, resulting in a final score for each starter vis-à-vis each 
opponent. These final values can thus range between 0 and 4 again
and can be summed over the course of the game, which 
is exactly what is plotted in the above line chart.


- Crossroads:
There were both literal and figurative crossroads in preparing the data.
At some points in the game there are different ways one could go
without much discrepancy in speed to complete the game (the chosen
criteria for which fights to include in the data).In fact, it may even
depend on the starter which route is the fastest, therefore these
decisions would affect the dis-/advantage values, as one would face 
different opponents. But these occasions are rare and can not
change the overall picture. My decision was to take the route the
above-mentioned speedrun took without taking into account more
beneficial routes for a specific starter.
Another decision involved the consideration of type-specific moves.
Nidoking, the final evolution of Nidoran♂ can learn moves of up 
to 9 different types. However, it can only "store" 4 moves at a
time. Although Pokémon can unlearn moves of a specific type and
relearn moves of the same type later, it is not possible to teach
just the right moves for the next opponent and re-configure them
afterwards. As this part is quite complicated and different
solutions are possible of when to learn and unlearn each move,
I decided to determine only an "entry-point" at which a starter
can theoretically learn a move of a specific type and kept it as 
a constant attribute until the end of the game. Yet another
involves Nidoran♂. This one is catchable no matter which
"real" starter is chosen at the start. In the above speedrun
the choice is Squirtle as it has profound advantage in the 
first few fights and cna therefore support Nidoran♂ at that
time until it has grown enough to fight on its own. However,
this choice slightly affects the Pokémon your rival uses,
which you fight several times during the game, therefore this
could also alter Nidoran♂´s score. Nevertheless, I believe 
that the difference is probably +/-3 and therefore regarded it
as nearly irrelevant. I determined Nidoran♂´s value choosing
Squirtle at the beginning, just like the above speedrun did. 






