AI as AI designer: Fine-Tuning Utility Values Using Genetic Algorithms


Self designing, Utility based AI using an open-source Bomberman clone by Drummyfish.


Make sure there is no PLAYER, and only AI, or the simulation will crash.



Overview:

We used a genetic algorithms to fine-tune how the values of a Utility-based AI are generated so that the optimal behavior eventually emerges.



Novelty: 

Using the genetic algorithm, we are letting the AI design itself and perfecting the utility curves for us. This has never been done before.


Value:
As it stands right now, designers have to spend hours painstakingly adjusting the curves of utility algorithms manually if they want to customize the behavior of the NPC they are designing. Our approach would allow them to specify a desired behavior and our AI would then proceed to adjust the utility curves as needed to produce that behavior.



Team Members: Nicholas Ho, Andrew Gwinner, Chris Huynh	

Technology: 
We found an open-source version of Bomberman that is already built entirely in Python. We had to do some casting of floats to integers as the codebase was built on Python 2.7.  Once we took care of that, the game finally stopped crashing. Our version runs on 3.2 which was the most up to date version of Pygame we could find. Pygame is needed for the game to run.

The limited AI that is built into the game is only allowed to update whenever a random amount of time has elapsed. The comments of the original creator of this code say this “saves CPU time and prevents jerky AI movement” but it also creates a very unsatisfying opponent to play against. The way it gets away with only calling AI so infrequently is that it makes the AI repeat the actions it computed until it gets called to update its instructions again. This leads to cases where the AI walks back and forth doing nothing in one direction, computes, then walks back and forth perpendicular to its original useless path.

We developed the AI to use utility to navigate the map and determine its action by getting data from their opponents and hazards to adapt to situations and determine what is most effective. Each action the AI is allowed to perform (throw bomb, place bomb, push bomb, no action, etc.) has it’s own unique curve that generates the priority based off various variables gathered from the state of the game. A* was also implemented for the AI’s pathfinding. A movement and action can be done each frame so we have each in a separate queue to accommodate this. In most situations the action queue decides not to place a bomb due to a plethora of inputs so while the player can move and perform an action at the same time it is also unlikely to occur too often.

The variables that transform our utility curves are stored in the array our genetic algorithm deals with. The fitness function of our genetic algorithm favors AI that lives the longest and provides more meaningful actions. The top three winners populate the next generation by using crossover technique. Our goal is that over the course of many generations, an AI will emerge with the correctly tweaked variables to create a fun and formidable adversary. If given more time, we planned to create a more timid foe through a more sophisticated fitness function. Also, we would have added a random mutator in addition to the crossover. This way we can ensure that each outcome will be unique and be used as a fallback if the behaviors in the current round are all unsatisfactory.

When utilizing our genetics algorithm, we ran ten different sets of ten generations. The initial population of each set were randomly generated using different value ranges, i.e. in one iteration we created a random boundary value and produced a random value within that range. To assist in creating an optimal AI, we picked out the greatest performers in the tenth generation of each set and ran the same genetics algorithm on it, creating the most optimal AI. When comparing the first generation of each set to our most optimal AI, we see a significant improvement. In the first generation of each set, the AI clustered on top of other AIs and provoked a mass explosion which killed all AIs. In contrast, the optimal AI separated and were able to dodge explosions more efficiently.

The programmer even though they calculated danger in the program never put it onto the “danger_map” that they programmed. With this addition, the AI’s are able to know when a tile will be dangerous as opposed to whether it is just dangerous in general. Additionally this allowed for the danger rating of a tile which the program calculates as between 0 and 3000. Putting this on y = (1/1.001)^x yields as our base curves gives a priority between 0.6 and 1. 

In the original finite state machine implementation of the AI code there was an if else block that increased the likelihood a bomb was dropped as less boxes were on the field. Putting that ratio onto y=(.2*(-(x-.3)^{(1/3)}))+.3 approximated the step function of the if else block nicely but also allowed for more finesse closer to 0 and 1. This function ended up getting sidelined by another function y = x^.5 which produced smarter bomb placement. However we found that we could actually get even better results and bring that function into the fold by making the new curve raised to the power of the old curve instead of 0.5 This reduces an index in the array in which we were storing our alterable transform values but it justified the existence of the other curve, thus preserving two slots. We wanted to maximize the number of values the genetics function could tweak.

