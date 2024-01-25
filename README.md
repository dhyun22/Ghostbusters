# Assignment 3: Ghostbusters
## Question 1 (2 points): Bayes Net Structure
an empty Bayes Net with the structure described below. 
A Bayes Net is incomplete without the actual probabilities, but factors are defined and assigned by staff code separately; you don’t need to worry about it. 
If you are curious, you can take a look at an example of how it works in printStarterBayesNet in bayesNet.py. 
Reading this function can also be helpful for doing this question.
## Question 2 (3 points): Join Factors
Implement the joinFactors function in factorOperations.py. 
It takes in a list of Factors and returns a new Factor whose probability entries are the product of the corresponding rows of the input Factors.
## Question 3 (2 points): Eliminate (not ghosts yet)
Implement the eliminate function in factorOperations.py. It takes a Factor and a variable to eliminate and returns a new Factor that does not contain that variable. 
This corresponds to summing all of the entries in the Factor which only differ in the value of the variable being eliminated.
## Question 4 (2 points): Variable Elimination
Implement the inferenceByVariableElimination function in inference.py. 
It answers a probabilistic query, which is represented using a BayesNet, a list of query variables, and the evidence.
## Question 5a: DiscreteDistribution Class
- First, fill in the `normalize` method, which normalizes the values in the distribution to sum to one, but keeps the proportions of the values the same. Use the `total` method to find the sum of the values in the distribution. For an empty distribution or a distribution where all of the values are zero, do nothing.
  Note that this method modifies the distribution directly, rather than returning a new distribution.
- Second, fill in the `sample` method, which draws a sample from the distribution, where the probability that a key is sampled is proportional to its corresponding value.
  Assume that the distribution is not empty, and not all of the values are zero. Note that the distribution does not necessarily have to be normalized prior to calling this method.
  You may find Python’s built-in `random.random()` function useful for this question.
## Question 5b (1 point): Observation Probability
In this question, you will implement the getObservationProb method in the InferenceModule base class in inference.py. 
This method takes in an observation (which is a noisy reading of the distance to the ghost), Pacman’s position, the ghost’s position, and the position of the ghost’s jail, and returns the probability of the noisy distance reading given Pacman’s position and the ghost’s position.
## Question 6 (3 points): Exact Inference Observation
In this question, you will implement the observeUpdate method in ExactInference class of inference.py to correctly update the agent’s belief distribution over ghost positions given an observation from Pacman’s sensors. 
You are implementing the online belief update for observing new evidence. 
The observeUpdate method should, for this problem, update the belief at every position on the map after receiving a sensor reading. 
You should iterate your updates over the variable self.allPositions which includes all legal positions plus the special jail position. 
Beliefs represent the probability that the ghost is at a particular location, and are stored as a DiscreteDistribution object in a field called self.beliefs, which you should update.
## Question 7 (3 points): Exact Inference with Time Elapse
In this question, you will implement the elapseTime method in ExactInference. The elapseTime step should, for this problem, update the belief at every position on the map after one time step elapsing. 
Your agent has access to the action distribution for the ghost through self.getPositionDistribution.
## Question 8 (1 point): Exact Inference Full Test
Implement the chooseAction method in GreedyBustersAgent in bustersAgents.py. 
Your agent should first find the most likely position of each remaining uncaptured ghost, then choose an action that minimizes the maze distance to the closest ghost.
## Question 9 (2 points): Approximate Inference Initialization and Beliefs
First, implement the functions initializeUniformly and getBeliefDistribution in the ParticleFilter class in inference.py. 
A particle (sample) is a ghost position in this inference problem. Note that, for initialization, particles should be evenly (not randomly) distributed across legal positions in order to ensure a uniform prior. 
We recommend thinking about how the mod operator is useful for initializeUniformly.
Note that the variable you store your particles in must be a list. A list is simply a collection of unweighted variables (positions in this case). 
Storing your particles as any other data type, such as a dictionary, is incorrect and will produce errors. 
The getBeliefDistribution method then takes the list of particles and converts it into a DiscreteDistribution object.
## Question 10 (3 points): Approximate Inference Observation
Next, we will implement the `observeUpdate` method in the `ParticleFilter` class in `inference.py`. This method constructs a weight distribution over `self.particles` where the weight of a particle is the probability of the observation given Pacman’s position and that particle location. Then, we resample from this weighted distribution to construct our new list of particles.

You should again use the function `self.getObservationProb` to find the probability of an observation given Pacman’s position, a potential ghost position, and the jail position. The sample method of the `DiscreteDistribution` class will also be useful. As a reminder, you can obtain Pacman’s position using `gameState.getPacmanPosition()`, and the jail position using `self.getJailPosition()`.

There is one special case that a correct implementation must handle. When all particles receive zero weight, the list of particles should be reinitialized by calling `initializeUniformly`. The total method of the `DiscreteDistribution` may be useful.
## Question 11 (3 points): Approximate Inference with Time Elapse
Implement the `elapseTime` function in the `ParticleFilter` class in `inference.py`. This function should construct a new list of particles that corresponds to each existing particle in `self.particles` advancing a time step, and then assign this new list back to `self.particles`. When complete, you should be able to track ghosts nearly as effectively as with exact inference.

Note that in this question, we will test both the `elapseTime` function in isolation, as well as the full implementation of the particle filter combining `elapseTime` and `observe`.
