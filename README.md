# Probing-_Adaptive-Forgetting-in-a-Meta-RL-Agent

This project is an extension of the "Reinforcement Learning, Fast and Slow" model. It investigates a more advanced cognitive concept: adaptive forgetting and is based on the 2022 based article: "Adaptive forgetting in the prefrontal cortex for flexible learning" by Feng et al.

**Core Idea**
While the first project demonstrated that the "fast" learning system (our RNN model) can track gradual changes in a volatile environment, this experiment tests how it responds to abrupt, structural shifts in the environment's rules. The central hypothesis is that an effective learning agent should not just adapt quickly, but also know when to rapidly discard outdated information following a major environmental change (a "changepoint"). This prevents interference from old, now-irrelevant knowledge and allows for rapid re-learning.

To test this, we use a technique called activation probing. We analyze the internal memory state (the LSTM hidden state) of the trained RNN agent to see if it dynamically represents the changing rules of the world. A sudden, sharp shift in the agent's internal representation immediately following a changepoint serves as direct evidence of this adaptive forgetting mechanism at work.

**My approach**
1. The Trained Agent
We begin with the "fast" MetaRLAgent that was fully trained on the "restless bandit" task in the first project. This agent has already learned a general strategy for adaptation.

2. The New World: A "Changepoint" Bandit
We introduce a new environment designed to test forgetting. Instead of slowly drifting probabilities, this world has abrupt changepoints:
For a stable block of 50 trials, one arm is clearly better. Then, at the "changepoint," the probabilities instantly flip. This creates a world of stable "rules" followed by sudden, drastic changes.

3. The "Mind-Reading" Probe
The core of this experiment is activation probing. We want to see if the agent's internal memory state reflects the abrupt changes in the world.
- Collect Data: We run the trained agent in the new changepoint environment. At every single step, we record the agent's internal "brain activity"—its LSTM hidden state.
- Train a Probe: We train a very simple linear regression model (the "probe").
   -Input: The agent's recorded brain activity.
   -Aim: Predict the environment's true reward probabilities at that moment.
-Analyze: If the simple probe can accurately predict the world's rules just by looking at the agent's brain activity, it proves the agent is actively representing that information in its memory.

**Final Result**
The final plot shows the probe's predictions (solid lines) almost perfectly tracking the true probabilities (dashed lines). At each changepoint (vertical dotted lines), the predictions make a sharp, rapid jump, mirroring the change in the environment. This is visual proof of adaptive forgetting: the agent's internal memory didn't slowly drift; it performed a rapid reset to accommodate the new rules.

**Technologies Used**
- Python 3.x
- TensorFlow 2.x
- NumPy
- Matplotlib
- Scikit-learn (for the linear regression probe)

**Citing the Original Work**
This project is an implementation based on the foundational ideas presented in the following papers:

- Core Concept: Wang, J. X., Kurth-Nelson, Z., Kumaran, D., et al. (2018). Prefrontal cortex as a meta-reinforcement learning system. Nature Neuroscience.
- Probing & Forgetting Inspiration: Feng, V., Wang, J. X., Kurth-Nelson, Z., & Botvinick, M. (2022). Adaptive forgetting in the prefrontal cortex for flexible learning. bioRxiv.

