# Q Learning Algorithm


## AIM
To develop a Python program to find the optimal policy for the given RL environment using Q-Learning and compare the state values with the Monte Carlo method.

## PROBLEM STATEMENT
The bandit slippery walk problem is a reinforcement learning problem in which an agent must learn to navigate a 7-state environment in order to reach a goal state. The environment is slippery, so the agent has a chance of moving in the opposite direction of the action it takes.The environment has 7 states where two of them are terminal states(Goal State and Hole State) and remaining five are transition stations including the starting state. The actions that the agent take are to move right and to move left.There are various transition probabilities for each action that the agent takes.50% chance that the agent moves in the intended direction.33.33% chance that the agent stays in its current state.16.66% chance that the agent moves in the opposite direction.
## Q LEARNING ALGORITHM
Step 1:
Initialize Q-table and hyperparameters.

Step 2:
Choose an action using the epsilon-greedy policy and execute the action, observe the next state, reward, and update Q-values and repeat until episode ends.

Step 3:
After training, derive the optimal policy from the Q-table.

Step 4:
Implement the Monte Carlo method to estimate state values.

Step 5:
Compare Q-Learning policy and state values with Monte Carlo results for the given RL environment.

## Q LEARNING FUNCTION
```
Developed by: M.Gunasekhar
Reg no: 212221240014
def q_learning(env, 
               gamma=1.0,
               init_alpha=0.5,
               min_alpha=0.01,
               alpha_decay_ratio=0.5,
               init_epsilon=1.0,
               min_epsilon=0.1,
               epsilon_decay_ratio=0.9,
               n_episodes=3000):
    nS, nA = env.observation_space.n, env.action_space.n
    pi_track = []
    Q = np.zeros((nS, nA), dtype=np.float64)
    Q_track = np.zeros((n_episodes, nS, nA), dtype=np.float64)
    select_action = lambda state,Q,epsilon: np.argmax(Q[state]) if np.random.random()>epsilon else np.random.randint(len(Q[state]))
    alphas=decay_schedule(init_alpha,min_alpha,alpha_decay_ratio,n_episodes)
    epsilons = decay_schedule(init_epsilon,min_epsilon,epsilon_decay_ratio,n_episodes)
    for e in tqdm(range(n_episodes),leave=False):
      state,done=env.reset(),False
      action=select_action(state,Q,epsilons[e])
      while not done:
        
        action=select_action(state,Q,epsilons[e])
        next_state,reward,done,_=env.step(action)
        td_target=reward+gamma*Q[next_state].max()*(not done)
        td_error=td_target-Q[state][action]
        Q[state][action]=Q[state][action]+alphas[e]*td_error
        state=next_state
      Q_track[e]=Q
      pi_track.append(np.argmax(Q,axis=1))
    V=np.max(Q,axis=1)
    pi=lambda s:{s:a for s,a in enumerate(np.argmax(Q,axis=1))}[s]
    return Q, V, pi, Q_track, pi_track
 ```
## OUTPUT:
![Screenshot 2024-05-05 230315](https://github.com/gunasekhar159/q-learning/assets/95043391/31f5d911-cb37-4dbb-971c-3d74c585c2a3)
![Screenshot 2024-05-05 230651](https://github.com/gunasekhar159/q-learning/assets/95043391/1d11dde0-d9c2-47c5-a2d2-6686e7362534)
![Screenshot 2024-05-05 230823](https://github.com/gunasekhar159/q-learning/assets/95043391/9ae8c532-b998-4a83-8731-c2f2e2169bfb)
![Screenshot 2024-05-05 230905](https://github.com/gunasekhar159/q-learning/assets/95043391/b9e4de56-67eb-4b40-bdbe-372ca26e18d4)

## RESULT:

Therefore a python program has been successfully developed to find the optimal policy for the given RL environment using Q-Learning and compared the state values with the Monte Carlo method.
