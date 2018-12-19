# Cyber Physical Systems Research Project - Colonel Blotto Games

## What are the Colonel Blotto Games (CBG)
Colonel Blotto Games is a type of zero-sum game where the players, usually called colonels, have to distribute their limited number of resources over a series of battlefields inorder to win the game. THe player with the most resources on the battlefield wins that game. The payoff of the game is the number of battlefields won. The concept of CBG has many real world applications such as in smart grids and controllers.

In this version there are a series of physical nodes and cyber nodes. The physical nodes are connected through the cyber nodes. The user can only access the physical nodes through the cyber nodes. Each physical node is connected to either 1 or more cyber nodes. If the attacker to the networked system manages to take down one of the cyber nodes then all the physical nodes attached to it are knocked out.

## How the files work
### ChangingresourceMain.m
At this link you can download the [file](ChangingresourceMain.m). In this source code, it defines the input parameters for the other 3 function programs. It initially calls resourcecombos.m to create resource distribution matrices. Then, it calls the Gamebuild.m to build the payoff matrix from the resources distribution matrices and other input parameters. Next, the npg2.m file is called to solve the Nash Equilibrium of the givengame parameters based on the payoff matrices. The output of the ChangingresourceMain.m file is a graph of defender resources vs. the value of teh game.

### Gamebuild.m
At this link you can download the [file](Gamebuild.m). This source code has inputs:
- na = number of cyber nodes
- CA = resource distribution matrices
- A = connection matrix to physical nodes
- Cost = Cost/payoff of each physical node to each player
- V = Number of cyber nodes needed for attacker to bring down a physical node
What this file does is it iterates through each strategy set and compares the combined resources of the attacker to the resources of the defender. From there it determines the wineer of each cyber node and uses the A and V inputs to determine if any physical nodes are down. The output of the code is a payoff matrix for each player from the cost input and the the nodes that are down.

### npg2.m
At this link you can download the [file](npg2.m). This source code has inputs:
- M:length of each strategy set
- U: payoff matrices
This code determines the Nash Equilibrium of the game from teh given payoff matrices. The possible downside to this program is it only returns one possible Nash Equilibrium in a given game even if the game may have more than one.

### resourcecombos.m
At this link you can download the [file](resourcecombos.m). This source code has inputs:
- na: number of cyber nodes
- x: number of player resources
As well as a 2 by na matrix that controls how the CBG system is networked. This program determines which resource values are needed based on the number of resources the player has. It then distributes those resources over the cyber nodes and returns distribution matrices in valve CA for each player.

## The Intended Changes 
The changes and analyses I intend to make:
1. Main Goal: Increase the speed in which the MATLAB files make the calculations. Either keep them as MATLAB files or using python to help improve the speed.
2. Check that the files run correctly.
3. Try to add more than 1 "A" vector. Analyze: Who does this benefit more: the attacker or the defender?
4. How does the equilibrium change the cost values? How does the equilibrium change?

## Supporting Papers
Supporting Papers:
This is the link to a basic paper on [Colonel Blotto](Gamebuild.m)
Here is another link to a more comprehensive paper on [Colonel Blotto Games](https://arxiv.org/pdf/1610.02110.pdf).
For a more general understanding of Colonel Blotto Games and how they work [wikipedia](https://en.wikipedia.org/wiki/Blotto_game) also has a good description.

## Current Progress
The current timing of the original files is . In order to reduce the amount of time it takes the program loops either need to be replaced with matrix calculations or find some other way to do the calculations. I have located the gamer.m function that the original user used in his program to do the Nash Equilibrium calculations.



## 
