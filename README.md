Download Link: https://assignmentchef.com/product/solved-ve492-project-3-reinforcement-learning
<br>
The goal of this project is to help you better understand reinforcement learning algorithms.

<h1>Introduction</h1>

In this project, you will implement value iteration and Q-learning. You will test your agents first on Gridworld (from class), then apply them to a simulated robot controller (Crawler) and Pacman.

The code for this project consists of several Python files, some of which you will need to read and understand in order to complete the assignment, and some of which you can ignore. You can download all the code and supporting files as a <a href="https://umjicanvas.com/files/222322/download?download_frd=1">zip archive</a> in the folder VE492 Projects on Canvas.

<table width="539">

 <tbody>

  <tr>

   <td colspan="2" width="539"><strong>Files you’ll edit:</strong></td>

  </tr>

  <tr>

   <td width="238">valueIterationAgents.py</td>

   <td width="301">A value iteration agent for solving known MDPs.</td>

  </tr>

  <tr>

   <td width="238">qlearningAgents.py</td>

   <td width="301">Q-learning agents for Gridworld, Crawler and Pacman.</td>

  </tr>

  <tr>

   <td width="238">analysis.py</td>

   <td width="301">A file to put your answers to questions given in the project.</td>

  </tr>

  <tr>

   <td colspan="2" width="539"><strong>Files you should read but NOT edit:</strong></td>

  </tr>

  <tr>

   <td width="238">mdp.py</td>

   <td width="301">Defines methods on general MDPs.</td>

  </tr>

  <tr>

   <td width="238">learningAgents.py</td>

   <td width="301">Defines the base classes ValueEstimationAgent and QLearningAgent, which your agents will extend.</td>

  </tr>

  <tr>

   <td width="238">util.py</td>

   <td width="301">Utilities, including util.Counter, which is particularly useful for Q-learners.</td>

  </tr>

  <tr>

   <td width="238">gridworld.py</td>

   <td width="301">The Gridworld implementation.</td>

  </tr>

  <tr>

   <td width="238">featureExtractors.py</td>

   <td width="301">Classes for extracting features on (state, action) pairs. Used for the approximateQ-learning agent (in qlearningAgents.py).</td>

  </tr>

  <tr>

   <td colspan="2" width="539"><strong>Files you can ignore:</strong></td>

  </tr>

  <tr>

   <td width="238">environment.py</td>

   <td width="301">Abstract class for general reinforcement learning environments.Used by gridworld.py.</td>

  </tr>

  <tr>

   <td width="238">graphicsGridworldDisplay.py</td>

   <td width="301">Gridworld graphical display.</td>

  </tr>

  <tr>

   <td width="238">graphicsUtils.py</td>

   <td width="301">Graphics utilities.</td>

  </tr>

  <tr>

   <td width="238">textGridworldDisplay.py</td>

   <td width="301">Plug-in for the Gridworld text interface.</td>

  </tr>

  <tr>

   <td width="238">crawler.py</td>

   <td width="301">The crawler code and test harness. You will run this but not edit it.</td>

  </tr>

  <tr>

   <td width="238">graphicsCrawlerDisplay.py</td>

   <td width="301">GUI for the crawler robot.</td>

  </tr>

 </tbody>

</table>

<strong>Getting Help: </strong>You are not alone! If you find yourself stuck on something, contact the course staff for help. Office hours and the discussion forum on Piazza are there for your support; please use them. If you can’t make our office hours, let us know and we will schedule more. We want these projects to be rewarding and instructional, not frustrating and demoralizing. But, we don’t know when or how to help unless you ask.

<strong>Discussion:      </strong>Please be careful not to post spoilers.

<h1>MDPs</h1>

To get started, run Gridworld in manual control mode, which uses the arrow keys:

python gridworld.py -m

You will see the two-exit layout from class. The blue dot is the agent. Note that when you press <em>up</em>, the agent only actually moves north 80% of the time. Such is the life of a Gridworld agent!

You can control many aspects of the simulation. A full list of options is available by running: python gridworld.py -h The default agent moves randomly python gridworld.py -g MazeGrid

You should see the random agent bounce around the grid until it happens upon an exit. Not the finest hour for an AI agent.

Note: The Gridworld MDP is such that you first must enter a pre-terminal state (the double boxes shown in the GUI) and then take the special ‘exit’ action before the episode actually ends (in the true terminal state called TERMINAL_STATE, which is not shown in the GUI). If you run an episode manually, your total return may be less than you expected, due to the discount rate (-d to change; 0.9 by default).

Look at the console output that accompanies the graphical output (or use -t for all text). You will be told about each transition the agent experiences (to turn this off, use -q).

As in Pacman, positions are represented by (x,y) Cartesian coordinates and any arrays are indexed by [x][y], with ‘north’ being the direction of increasing y, etc. By default, most transitions will receive a reward of zero, though you can change this with the living reward option (-r).

<h1>Question 1  Value Iteration</h1>

Recall the value iteration state update equation:

Write a value iteration agent in ValueIterationAgent, which has been partially specified for you in valueIterationAgents.py. Your value iteration agent is an offline planner, not a reinforcement learning agent, and so the relevant training option is the number of iterations of value iteration it should run (option -i) in its initial planning phase. ValueIterationAgent takes an MDP on construction and runs value iteration for the specified number of iterations before the constructor returns.

Value iteration computes <em>k</em>-step estimates of the optimal values,<em>V<sub>k</sub></em>. In addition to running value iteration, implement the following methods for ValueIterationAgent using <em>V<sub>k</sub></em>.

<ul>

 <li>computeActionFromValues(state) computes the best action according to the value function given by values.</li>

 <li>computeQValueFromValues(state, action) returns the Q-value of the (state, action) pair given by the value function given by values.</li>

</ul>

These quantities are all displayed in the GUI: values are numbers in squares, Q-values are numbers in square quarters, and policies are arrows out from each square.

<em>Important: </em>Use the “batch” version of value iteration where each vector <em>V<sub>k </sub></em>is computed from a fixed vector <em>V<sub>k</sub></em><sub>−<em>1</em></sub>, not the “online” version where one single weight vector is updated in place. This means that when a state’s value is updated in iteration <em>k </em>based on the values of its successor states, the successor state values used in the value update computation should be those from iteration <em>k</em>-1 (even if some of the successor states had already been updated in iteration <em>k</em>).

<em>Note: </em>A policy synthesized from values of depth <em>k </em>(which reflect the next <em>k </em>rewards) will actually reflect the next <em>k</em>+1 rewards (i.e. you return <em>π<sub>k</sub></em><sub>+<em>1</em></sub>). Similarly, the Q-values will also reflect one more reward than the values (i.e. you return <em>Q<sub>k</sub></em><sub>+<em>1</em></sub>).

You should return the synthesized policy <em>π<sub>k</sub></em><sub>+<em>1</em></sub>.

<em>Hint: </em>You may optionally use the util.Counter class in util.py, which is a dictionary with a default value of zero. However, be careful with argMax: the actual argmax you want may be a key not in the counter!

<em>Note: </em>Make sure to handle the case when a state has no available actions in an MDP (think about what this means for future rewards).

The following command loads your ValueIterationAgent, which will compute a policy and execute it 10 times. Press a key to cycle through values, Q-values, and the simulation. You should find that the value of the start state (V(start), which you can read off of the GUI) and the empirical resulting average reward (printed after the 10 rounds of execution finish) are quite close.

python gridworld.py -a value -i 100 -k 10

<em>Hint: </em>On the default BookGrid, running value iteration for 5 iterations should give you this output: python gridworld.py -a value -i 5

<em>Grading: </em>Your value iteration agent will be graded on a new grid. We will check your values, Q-values, and policies after fixed numbers of iterations and at convergence (e.g. after 100 iterations).

<h1>Question 2 : Bridge Crossing Analysis</h1>

BridgeGrid is a grid world map with the a low-reward terminal state and a high-reward terminal state separated by a narrow “bridge”, on either side of which is a chasm of high negative reward. The agent starts near the lowreward state. With the default discount of 0.9 and the default noise of 0.2, the optimal policy does not cross the bridge. Change only ONE of the discount and noise parameters so that the optimal policy causes the agent to attempt to cross the bridge. Put your answer in question2() of analysis.py. (Noise refers to how often an agent ends up in an unintended successor state when they perform an action.) The default corresponds to:

python gridworld.py -a value -i 100 -g BridgeGrid –discount 0.9

–noise 0.2

<em>Grading:</em>We will check that you only changed one of the given parameters, and that with this change, a correct value iteration agent should cross the bridge.

<h1>Question 3 ): Policies</h1>

Consider the DiscountGrid layout, shown below. This grid has two terminal states with positive payoff (in the middle row), a close exit with payoff +1 and a distant exit with payoff +10. The bottom row of the grid consists of terminal states with negative payoff (shown in red); each state in this “cliff” region has payoff -10. The starting state is the yellow square. We distinguish between two types of paths: (1) paths that “risk the cliff” and travel near the bottom row of the grid; these paths are shorter but risk earning a large negative payoff, and are represented by the red arrow in the figure below. (2) paths that “avoid the cliff” and travel along the top edge of the grid. These paths are longer but are less likely to incur huge negative payoffs. These paths are represented by the green arrow in the figure below.

In this question, you will choose settings of the discount, noise, and living reward parameters for this MDP to produce optimal policies of several different types. <strong>Your setting of the parameter values for each part should have the property that, if your agent followed its optimal policy without being subject to any noise, it would exhibit the given behavior. </strong>If a particular behavior is not achieved for any setting of the parameters, assert that the policy is impossible by returning the string

‘NOT POSSIBLE’.

Here are the optimal policy types you should attempt to produce:

<ol>

 <li>Prefer the close exit (+1), risking the cliff (-10)</li>

 <li>Prefer the close exit (+1), but avoiding the cliff (-10)</li>

 <li>Prefer the distant exit (+10), risking the cliff (-10)</li>

 <li>Prefer the distant exit (+10), avoiding the cliff (-10)</li>

 <li>Avoid both exits and the cliff (so an episode should never terminate)</li>

</ol>

question3a() through question3e() should each return a 3-item tuple of (discount, noise, living reward) in analysis.py.

<em>Note: </em>You can check your policies in the GUI. For example, using a correct answer to 3(a), the arrow in (0,1) should point east, the arrow in (1,1) should also point east, and the arrow in (2,1) should point north.

<em>Note: </em>On some machines you may not see an arrow. In this case, press a button on the keyboard to switch to qValue display, and mentally calculate the policy by taking the arg max of the available qValues for each state. <em>Grading: </em>We will check that the desired policy is returned in each case.

<h1>Question 4 : Asynchronous Value Iteration</h1>

Write a value iteration agent in AsynchronousValueIterationAgent, which has been partially specified for you in valueIterationAgents.py. Your value iteration agent is an offline planner, not a reinforcement learning agent, and so the relevant training option is the number of iterations of value iteration it should run (option -i) in its initial planning phase.

AsynchronousValueIterationAgent takes an MDP on construction and runs cyclic value iteration (described in the next paragraph) for the specified number of iterations before the constructor returns. Note that all this value iteration code should be placed inside the constructor (__init__ method). The reason this class is called AsynchronousValueIterationAgent is because we will update only <strong>one </strong>state in each iteration, as opposed to doing a batch-style update. Here is how cyclic value iteration works. In the first iteration, only update the value of the first state in the states list. In the second iteration, only update the value of the second. Keep going until you have updated the value of each state once, then start back at the first state for the subsequent iteration. <strong>If the state picked for updating is terminal, nothing happens in that iteration. </strong>You can implement it as indexing into the states variable defined in the code skeleton.

As a reminder, here’s the value iteration state update equation:

Value iteration iterates a fixed-point equation, as discussed in class. It is also possible to update the state values in different ways, such as in a random order (i.e., select a state randomly, update its value, and repeat) or in a batch style (as in Q1). In this question, we will explore another technique.

AsynchronousValueIterationAgent inherits from ValueIterationAgent from Q1, so the only method you need to implement is runValueIteration. Since the superclass constructor calls runValueIteration, overriding it is sufficient to change the agent’s behavior as desired.

<em>Note: </em>Make sure to handle the case when a state has no available actions in an MDP (think about what this means for future rewards).

The following command loads your AsynchronousValueIterationAgent in the Gridworld, which will compute a policy and execute it 10 times. Press a key to cycle through values, Q-values, and the simulation. You should find that the value of the start state (V(start), which you can read off of the GUI) and the empirical resulting average reward (printed after the 10 rounds of execution finish) are quite close.

python gridworld.py -a asynchvalue -i 1000 -k 10

<em>Grading: </em>Your value iteration agent will be graded on a new grid. We will check your values, Q-values, and policies after fixed numbers of iterations and at convergence (e.g., after 1000 iterations).

<h1>Question 5 : Prioritized Sweeping Value Iteration</h1>

You will now implement PrioritizedSweepingValueIterationAgent, which has been partially specified for you in valueIterationAgents.py. Note that this class derives from AsynchronousValueIterationAgent, so the only method that needs to change is runValueIteration, which actually runs the value iteration.

Prioritized sweeping attempts to focus updates of state values in ways that are likely to change the policy.

For this project, you will implement a simplified version of the standard prioritized sweeping algorithm, which is described in <a href="https://umjicanvas.com/files/222318/download?download_frd=1">this paper</a><a href="https://umjicanvas.com/files/222318/download?download_frd=1">.</a> We’ve adapted this algorithm for our setting. First, we define the <strong>predecessors </strong>of a state s as all states that have a <strong>nonzero </strong>probability of reaching s by taking some action a. Also, theta, which is passed in as a parameter, will represent our tolerance for error when deciding whether to update the value of a state. Here’s the algorithm you should follow in your implementation.

<ul>

 <li>Compute predecessors of all states.</li>

 <li>Initialize an empty priority queue.</li>

 <li>For each non-terminal state s, do: (<strong>note: to make the autograder work for this question, you must iterate over states in the order returned by </strong>mdp.getStates())

  <ul>

   <li>Find the absolute value of the difference between the current value of s in values and the highest Q-value across all possible actions from s (this represents what the value should be); call this number diff. Do NOT update self.values[s] in this step.</li>

   <li>Push s into the priority queue with priority -diff (note that this is <strong>negative</strong>). We use a negative because the priority queue is a min heap, but we want to prioritize updating states that have a <strong>higher </strong></li>

  </ul></li>

 <li>For iteration in 0, 1, 2, …, self.iterations – 1, do:

  <ul>

   <li>If the priority queue is empty, then terminate.</li>

   <li>Pop a state s off the priority queue.</li>

   <li>Update the value of s (if it is not a terminal state) in values.</li>

   <li>For each predecessor p of s, do:</li>

  </ul></li>

</ul>

∗ Find the absolute value of the difference between the current value of p in self.values and the highest Q-value across all possible actions from p (this represents what the value should be); call this number diff. Do NOT update self.values[p] in this step.

∗ If diff &gt; theta, push p into the priority queue with priority -diff (note that this is <strong>negative</strong>), as long as it does not already exist in the priority queue with equal or lower priority. As before, we use a negative because the priority queue is a min heap, but we want to prioritize updating states that have a <strong>higher </strong>error.

A couple of important notes on implementation:

<ul>

 <li>When you compute predecessors of a state, make sure to store them in a <strong>set</strong>, not a list, to avoid duplicates.</li>

 <li>Please use PriorityQueue in your implementation. The update method in this class will likely be useful; look at its documentation.</li>

</ul>

You can run the PrioritizedSweepingValueIterationAgent in the Gridworld using the following command.

python gridworld.py -a priosweepvalue -i 1000

<em>Grading: </em>Your prioritized sweeping value iteration agent will be graded on a new grid. We will check your values, Q-values, and policies after fixed numbers of iterations and at convergence (e.g., after 1000 iterations).

<h1>Question 6 : Q-Learning</h1>

Note that your value iteration agent does not actually learn from experience. Rather, it ponders its MDP model to arrive at a complete policy before ever interacting with a real environment. When it does interact with the environment, it simply follows the precomputed policy (e.g. it becomes a reflex agent). This distinction may be subtle in a simulated environment like a Gridword, but it’s very important in the real world, where the real MDP is not available.

You will now write a Q-learning agent, which does very little on construction, but instead learns by trial and error from interactions with the environment through its update(state, action, nextState, reward) method. A stub of a Q-learner is specified in QLearningAgent in qlearningAgents.py, and you can select it with the option ‘-a q’. For this question, you must implement the update, computeValueFromQValues, getQValue, and computeActionFromQValues methods.

<em>Note: </em>For computeActionFromQValues, you should break ties randomly for better behavior. The random.choice() function will help. In a particular state, actions that your agent hasn’t seen before still have a Q-value, specifically a Q-value of zero, and if all of the actions that your agent has seen before have a negative Q-value, an unseen action may be optimal.

<em>Important: </em>Make sure that in your computeValueFromQValues and computeActionFromQValues functions, you only access Q values by calling getQValue. This abstraction will be useful for question 10 when you override getQValue to use features of state-action pairs rather than state-action pairs directly. With the Q-learning update in place, you can watch your Q-learner learn under manual control, using the keyboard: python gridworld.py -a q -k 5 -m

Recall that -k will control the number of episodes your agent gets to learn. Watch how the agent learns about the state it was just in, not the one it moves to, and “leaves learning in its wake.” Hint: to help with debugging, you can turn off noise by using the –noise 0.0 parameter (though this obviously makes Q-learning less interesting). If you manually steer Pacman north and then east along the optimal path for four episodes, you should see the following Q-values:

<em>Grading: </em>We will run your Q-learning agent and check that it learns the same Q-values and policy as our reference implementation when each is presented with the same set of examples.

<h1>Question 7 : Epsilon Greedy</h1>

Complete your Q-learning agent by implementing epsilon-greedy action selection in getAction, meaning it chooses random actions an epsilon fraction of the time, and follows its current best Q-values otherwise. Note that choosing a random action may result in choosing the best action – that is, you should not choose a random sub-optimal action, but rather any random legal action. You can choose an element from a list uniformly at random by calling the random.choice function. You can simulate a binary variable with probability p of success by using util.flipCoin(p), which returns True with probability p and False with probability 1-p.

After implementing the getAction method, observe the following behavior of the agent in gridworld (with epsilon = 0.3).

python gridworld.py -a q -k 100

Your final Q-values should resemble those of your value iteration agent, especially along well-traveled paths. However, your average returns will be

lower than the Q-values predict because of the random actions and the initial learning phase.

You can also observe the following simulations for different epsilon values. Does that behavior of the agent match what you expect? python gridworld.py -a q -k 100 –noise 0.0 -e 0.1 python gridworld.py -a q -k 100 –noise 0.0 -e 0.9

With no additional code, you should now be able to run a Q-learning crawler robot:

python crawler.py

If this doesn’t work, you’ve probably written some code too specific to the GridWorld problem and you should make it more general to all MDPs.

This will invoke the crawling robot from class using your Q-learner. Play around with the various learning parameters to see how they affect the agent’s policies and actions. Note that the step delay is a parameter of the simulation, whereas the learning rate and epsilon are parameters of your learning algorithm, and the discount factor is a property of the environment.

<h1>Question 8 Bridge Crossing Revisited</h1>

First, train a completely random Q-learner with the default learning rate on the noiseless BridgeGrid for 50 episodes and observe whether it finds the optimal policy.

python gridworld.py -a q -k 50 -n 0 -g BridgeGrid -e 1

Now try the same experiment with an epsilon of 0. Is there an epsilon and a learning rate for which it is highly likely (greater than 99%) that the optimal policy will be learned after 50 iterations? question8() in analysis.py should return EITHER a 2-item tuple of (epsilon, learning rate) OR the string ‘NOT POSSIBLE’ if there is none. Epsilon is controlled by -e, learning rate by -l.

<em>Note: </em>Your response should be not depend on the exact tie-breaking mechanism used to choose actions. This means your answer should be correct even if for instance we rotated the entire bridge grid world 90 degrees.

<h1>Question 9 : Q-Learning and Pacman</h1>

Time to play some Pacman! Pacman will play games in two phases. In the first phase, training, Pacman will begin to learn about the values of positions and actions. Because it takes a very long time to learn accurate Q-values even for tiny grids, Pacman’s training games run in quiet mode by default, with no GUI (or console) display. Once Pacman’s training is complete, he will enter testing mode. When testing, Pacman’s self.epsilon and self.alpha will be set to 0.0, effectively stopping Q-learning and disabling exploration, in order to allow Pacman to exploit his learned policy. Test games are shown in the GUI by default. Without any code changes you should be able to run Q-learning Pacman for very tiny grids as follows:

python pacman.py -p PacmanQAgent -x 2000 -n 2010 -l smallGrid

Note that PacmanQAgent is already defined for you in terms of the QLearningAgent you’ve already written. PacmanQAgent is only different in that it has default learning parameters that are more effective for the Pacman problem (epsilon=0.05, alpha=0.2, gamma=0.8). You will receive full credit for this question if the command above works without exceptions and your agent wins at least 80% of the time. We will run 100 test games after the 2000 training games.

<em>Hint: </em>If your QLearningAgent works for gridworld.py and crawler.py but does not seem to be learning a good policy for Pacman on smallGrid, it may be because your getAction and/or computeActionFromQValues methods do not in some cases properly consider unseen actions. In particular, because unseen actions have by definition a Q-value of zero, if all of the actions that have been seen have negative Q-values, an unseen action may be optimal. Beware of the argmax function from util. Counter!

<em>Note: </em>If you want to watch 10 training games to see what’s going on, use the command:

python pacman.py -p PacmanQAgent -n 10 -l smallGrid -a numTraining=10

During training, you will see output every 100 games with statistics about how Pacman is faring. Epsilon is positive during training, so Pacman will play poorly even after having learned a good policy: this is because he occasionally makes a random exploratory move into a ghost. As a benchmark, it should take between 1000 and 1400 games before Pacman’s rewards for a 100 episode segment becomes positive, reflecting that he’s started winning more than losing. By the end of training, it should remain positive and be fairly high (between 100 and 350).

Make sure you understand what is happening here: the MDP state is the <em>exact </em>board configuration facing Pacman, with the now complex transitions describing an entire ply of change to that state. The intermediate game configurations in which Pacman has moved but the ghosts have not replied are <em>not </em>MDP states, but are bundled in to the transitions.

Once Pacman is done training, he should win very reliably in test games (at least 90% of the time), since now he is exploiting his learned policy.

However, you will find that training the same agent on the seemingly simple mediumGrid does not work well. In our implementation, Pacman’s average training rewards remain negative throughout training. At test time, he plays badly, probably losing all of his test games. Training will also take a long time, despite its ineffectiveness.

Pacman fails to win on larger layouts because each board configuration is a separate state with separate Q-values. He has no way to generalize that running into a ghost is bad for all positions. Obviously, this approach will not scale.

<h1>Question 10 : Approximate Q-Learning</h1>

Implement an approximate Q-learning agent that learns weights for features of states, where many states might share the same features. Write your implementation in ApproximateQAgent class in qlearningAgents.py, which is a subclass of PacmanQAgent.

<em>Note: </em>Approximate Q-learning assumes the existence of a feature function <em>f</em>(<em>s,a</em>) over state and action pairs, which yields a vector [<em>f</em><sub>1</sub>(<em>s,a</em>)<em>,…,f<sub>i</sub></em>(<em>s,a</em>)<em>, …,f<sub>n</sub></em>(<em>s,a</em>)] of feature values. We provide feature functions for you in featureExtractors.py. Feature vectors are util.Counter (like a dictionary) objects containing the non-zero pairs of features and values; all omitted features have value zero.

The approximate Q-function takes the following form:

where each weight <em>ω<sub>i </sub></em>is associated with a particular feature <em>f<sub>i</sub></em>(<em>s,a</em>). In your code, you should implement the weight vector as a dictionary mapping features (which the feature extractors will return) to weight values. You will update your weight vectors similarly to how you updated Q-values:

<em>ω<sub>i </sub></em>← <em>ω<sub>i </sub></em>+ <em>α </em>· <em>difference </em>· <em>f<sub>i</sub></em>(<em>s,a</em>) difference = (<em>r </em>+ <em>γ </em>max<em>Q</em>(<em>s</em><sup>0</sup><em>,a</em><sup>0</sup>)) − <em>Q</em>(<em>s,a</em>)

<em>a</em>0

Note that the <em>difference </em>term is the same as in normal Q-learning, and <em>r </em>is the experienced reward.

By default, ApproximateQAgent uses the IdentityExtractor, which assigns a single feature to every (state,action) pair. With this feature extractor, your approximate Q-learning agent should work identically to PacmanQAgent. You can test this with the following command:

python pacman.py -p ApproximateQAgent -x 2000 -n 2010 -l smallGrid <em>Important: </em>ApproximateQAgent is a subclass of QLearningAgent, and it therefore shares several methods like getAction. Make sure that your methods in QLearningAgent call getQValue instead of accessing Q-values directly, so that when you override getQValue in your approximate agent, the new approximate q-values are used to compute actions.

Once you’re confident that your approximate learner works correctly with the identity features, run your approximate Q-learning agent with our custom feature extractor, which can learn to win with ease:

python pacman.py -p ApproximateQAgent -a extractor=SimpleExtractor -x 50 -n 60 -l mediumGrid

Even much larger layouts should be no problem for your ApproximateQAgent (<em>warning: </em>this may take a few minutes to train):

python pacman.py -p ApproximateQAgent -a extractor=SimpleExtractor -x 50 -n 60 -l mediumClassic

If you have no errors, your approximate Q-learning agent should win almost every time with these simple features, even with only 50 training games. <em>Grading: </em>We will run your approximate Q-learning agent and check that it learns the same Q-values and feature weights as our reference implementation when each is presented with the same set of examples. <em>Congratulations! You have a learning Pacman agent!</em>


