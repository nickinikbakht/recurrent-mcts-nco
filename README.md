# Recurrent NCO + MCTS: A Fast, Smart Routing Solver

Hi there! Welcome to my proof-of-concept repository.

This project is the technical foundation for my PhD research proposal. It bridges the deep, strategic decision-making of **Monte Carlo Tree Search (MCTS)** with the lightning-fast execution of **Recurrent State Encoders**.

## The "Aha!" Moment (Why I Built This)

During my Master's thesis working with *CombOpt Zero*, I learned that MCTS is brilliant at solving complex routing problems (like the Traveling Salesman Problem) without getting trapped in local minima. But there was a major catch: running a standard, fully dynamic Graph Neural Network (which takes $O(N^2)$ time) inside a massive search tree is computationally crushing. It's simply too slow for real-time industrial logistics.

Inspired by recent breakthroughs in latency-aware NCO from the **Information Systems and Machine Learning Lab (ISMLL)**, I realized the solution: **Cache the state.** By replacing the heavy graph calculations with a lightweight Recurrent Encoder inside the tree nodes, we can maintain the strategic lookahead of AlphaZero-style algorithms without the crippling latency.

## How It Works Under the Hood

1. **The Base Encoder (The Heavy Lifter):** A Transformer-based encoder that processes the entire graph from scratch ($O(N^2)$). To keep things fast, we only trigger this at the root node or every $k$ steps to prevent error accumulation.

2. **The Recurrent Encoder (The Speedster):** This is where the magic happens. As the MCTS expands its tree, this update gate takes the parent node's memory and applies a lightning-fast $O(N)$ update for the child node.

3. **Self-Critical Policy Gradient:** A robust Reinforcement Learning training loop I implemented to ensure the model can actually handle massive TSP-100 graphs without variance collapse.

## Run the Tests (Verify the 4.6x Speedup!)

I have consolidated the entire pipeline—models, MCTS integration, and tests—into one clean file: `Recurrent_NCO_MCTS_Clean.ipynb`.

If you want to verify the latency improvements yourself:

1. Clone this repository and open the Jupyter Notebook.

2. Run **Sections 1 & 2** to initialize the neural network classes.

3. Run **Section 4: Latency Benchmarking**. This acts as the primary test script, generating a dummy graph of 100 cities and pitting the Fully Dynamic model directly against the Recurrent Update model.

### Expected Test Output:

```text
Fully Dynamic Latency: 1.5849 sec
Recurrent Model Latency: 0.3428 sec
-> Speedup: 4.6x
```

## Requirements

* Python 3.8+
* PyTorch 2.0+

## Let's Connect

**Fatemeh Nikbakht**
[Insert your LinkedIn URL] | [Insert your Email]