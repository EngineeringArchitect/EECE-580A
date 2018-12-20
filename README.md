# Cyber Physical Systems Research Project - Colonel Blotto Games

## What are the Colonel Blotto Games (CBG)
Colonel Blotto Games is a type of zero-sum game where the players, usually called colonels, have to distribute their limited number of resources over a series of battlefields inorder to win the game. THe player with the most resources on the battlefield wins that game. The payoff of the game is the number of battlefields won. The concept of CBG has many real world applications such as in smart grids and controllers.

In this version there are a series of physical nodes and cyber nodes. The physical nodes are connected through the cyber nodes. The user can only access the physical nodes through the cyber nodes. Each physical node is connected to either 1 or more cyber nodes. If the attacker to the networked system manages to take down one of the cyber nodes then all the physical nodes attached to it are knocked out.

## How the files work
### ChangingresourceMain.m
At this link you can download the [file](ChangingresourceMain.m). In this source code, it defines the input parameters for the other 3 function programs. It initially calls resourcecombos.m to create resource distribution matrices. Then, it calls the Gamebuild.m to build the payoff matrix from the resources distribution matrices and other input parameters. Next, the npg2.m file is called to solve the Nash Equilibrium of the given game parameters based on the payoff matrices. The output of the ChangingresourceMain.m file is a graph of defender resources vs. the value of the game.

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
In order to see how the current functions perform, I used the tic and toc functions of MATLAB. The way they work is as follows, you place the key word tic at the start of where you want to start timing your program. THis startst the internal timer. Then, place the key word toc where you wish to stop timing your program. It will output a result, in seconds, of how much time it took to run the program.

After testing this functionality a few times, and seeing the results varied each time, I decided to get the best result was to take an average of 10 runs of the program. The average run time of the files is 95.7859 seconds.

In order to reduce the amount of time it takes the program loops either need to be replaced with matrix calculations or find some other way to do the calculations. I have located the [gamer.m](gamer.m) function that the original user used in his program to do the Nash Equilibrium calculations in order to run these locations since in it not part of the MATLAB library. 

The best way I have found that to remove the loops from the files is to change them to matrix operations. The only downside to this is that depending on the complexity of the operations being done in the loop is how feasible it is. In order to remove all the loops you will need to preallocate the space for each matrix instead of just looping over all the dimensions. This may cause an issue on some computers if that person doesn't have enough space. The other issue is some of the calculations over each individual element of the array may be producing a varying sized matrix value as well. This will result multidimensional arrays that may dbe hard to utilize later for whatever calculation purpose the program needs it for. It will also produce an error that will say something around the incorrect matrix dimensions if you didn't allocate space for the right size matrix. The other issue with replaceing the moops with matrices is that many of them are there to use "if-then-else" statements on each element of the matrix it is looping over then produce some form of value after that. Currently, I am a bit wary of how replacing the loops will affect these statments. What could be done is to havethe code create the second to last matrix then only have have the one loop for whatever purpose it is needed for, this truly depending on each file of code.

The proess for removing loops from MATLAB code for a basic double nested loop is listed in the following steps with images:
This is the initial loop
```
L=(length(time)-1);
 for q=1:numPT
    for  h=1:L
    x(q,h)=SortDate{q+numPT*(h-1),6};
    y(q,h)=SortDate{q+numPT*(h-1),7};
    c(q,h)=SortDate{q+numPT*(h-1),5};
    end
end
```
1. Pre-allocate the output of the final matrix, meaning filling the predetermined output matrix with zeros as place holders.
```
L = (length(time)-1);
x = zeros(numPT, L);
y = zeros(numPT, L);
c = zeros(numPT, L);
for q = 1:numPT
  for h=1:L
    x(q,h) = SortDate{q+numPT*(h-1),6};
    y(q,h) = SortDate{q+numPT*(h-1),7};
    c(q,h) = SortDate{q+numPT*(h-1),5};
  end
end
```
2. Next removing the inner loop. You change the column vector position of every place youa are comparaing to.
```
L = (length(time)-1);
x = zeros(numPT, L);
y = zeros(numPT, L);
c = zeros(numPT, L);
for q = 1:numPT
  x(q, :) = [SortDate{q+numPT*(0:L-1), 6}];  % Faster than: x(q, 1:L) = ...
  y(q, :) = [SortDate{q+numPT*(0:L-1), 7}];
  c(q, :) = [SortDate{q+numPT*(0:L-1), 5}];
end
```
3. Removing the outer loop is similar to the inner loop. Once you remove that, you no longer need to have the pre-allocation.
```
L = (length(time)-1);
index = (1:numPT).' + (0:numPT:numPT*(L-1)));  % >= R2016b
x = reshape(cell2mat(SortDate(index, 6)), size(index));
y = reshape(cell2mat(SortDate(index, 7)), size(index));
c = reshape(cell2mat(SortDate(index, 5)), size(index));
```

The above process was taken from: [here](https://www.mathworks.com/matlabcentral/answers/352068-replacing-the-for-loop-please).

The fix I see to this was possibly needing to pull some of these loops apart without taking away from the calculations purpose for those set of loops. The option for using another programing langange is also feasible but, for an any internal functions that were used in MATLAB you would either need to find a comparable function in that language or write your own.

