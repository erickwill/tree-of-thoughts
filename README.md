# Tree of Thoughts 🌳🌲🌴🌿🍃

Tree of Thoughts (ToT) is a powerful and flexible algorithm for leveraging pre-trained language models to solve various problems by exploring multiple reasoning paths. It's designed to be plug-and-play, allowing users to easily connect their models and use the Tree of Thoughts method.

## Getting started
Clone this repository with ```git clone https://github.com/kyegomez/tree-of-thoughts```

Navigate to the repository folder: ``` cd tree-of-thoughts```

```pip install openai transformers```

Create a Python script (e.g., example.py) and import the necessary classes:

``` 
from tree_of_thoughts import AbstractLanguageModel, TreeOfThoughts
import openai

# Replace 'your_api_key' with your actual OpenAI API key
api_key = 'your_api_key'

class OpenAILanguageModel(AbstractLanguageModel):
    def __init__(self):
        openai.api_key = api_key

    def generate_thoughts(self, state, k):
        # Implement thought generation logic using OpenAI's API
        pass

    def evaluate_states(self, states):
        # Implement state evaluation logic using OpenAI's API
        pass

# Create an instance of the OpenAILanguageModel
openai_model = OpenAILanguageModel()

# Choose a search algorithm: 'BFS' or 'DFS'
search_algorithm = 'BFS'

# Create an instance of the TreeOfThoughts class
tree_of_thoughts = TreeOfThoughts(openai_model, search_algorithm)

# Define your input problem and other required parameters
input_problem = "your_input_problem"
k = 5
T = 3
b = 5
vth = 0.5

# Call the solve method with the input problem and other required parameters
solution = tree_of_thoughts.solve(input_problem, k, T, b, vth)

# Print the solution
print(solution)
```

Replace the pass statements in the generate_thoughts and evaluate_states methods with the appropriate API calls and logic for your specific problem.

Run the example script:

``` python example.py```

🌟 Features:
- General problem-solving framework for language models
- Supports both breadth-first search (BFS) and depth-first search (DFS) algorithms
- Easy integration with popular language models like OpenAI and Hugging Face
- Extensible and adaptable to different problem properties and resource constraints

## Algorithmic Pseudocode

1. Define the thought decomposition based on the problem properties.
2. Create a thought generator function G(pθ, s, k) with two strategies:
   a. Sample i.i.d. thoughts from a CoT prompt.
   b. Propose thoughts sequentially using a "propose prompt".
3. Create a state evaluator function V(pθ, S) with two strategies:
   a. Value each state independently.
   b. Vote across states.
4. Choose a search algorithm (BFS or DFS) based on the tree structure.
5. Implement the chosen search algorithm.
6. Execute the chosen search algorithm with the input problem, thought generator, state evaluator, and other required parameters.

## Code v1
```
class TreeofThoughts:
    
    def __init__(self, model, thought_decomposition, thought_generator, state_evaluator, search_algorithm):
        self.model = model
        self.thought_decomposition = thought_decomposition
        self.thought_generator = thought_generator
        self.state_evaluator = state_evaluator
        self.search_algorithm = search_algorithm

    def solve(self, x, k, T, b, vth):
        if self.search_algorithm == 'BFS':
            return self.tot_bfs(x, k, T, b)
        elif self.search_algorithm == 'DFS':
            return self.tot_dfs(x, k, T, vth)
        else:
            raise ValueError("Invalid search algorithm. Choose 'BFS' or 'DFS'.")

    def tot_bfs(self, x, k, T, b):
        S0 = {x}
        for t in range(1, T + 1):
            S0_t = {(*s, z) for s in S0 for z in self.thought_generator(self.model, s, k)}
            Vt = self.state_evaluator(self.model, S0_t)
            St = sorted(S0_t, key=lambda s: Vt[s], reverse=True)[:b]
            S0 = set(St)
        return self.thought_generator(self.model, max(St, key=lambda s: Vt[s]), 1)

    def tot_dfs(self, x, k, T, vth):
        output = []

        def dfs(s, t):
            if t > T:
                output.append(self.thought_generator(self.model, s, 1))
                return
            for s_prime in sorted(self.thought_generator(self.model, s, k)):
                if self.state_evaluator(self.model, {s_prime})[s_prime] > vth:
                    dfs((*s, s_prime), t + 1)

        dfs(x, 1)
        return output
```


## Usage Examples

### OpenAI API

To use Tree of Thoughts with OpenAI's API, create a custom model class that inherits from `AbstractLanguageModel` and implements the required methods using OpenAI's API. Then, create an instance of the `TreeOfThoughts` class with the custom model and the desired search algorithm ('BFS' or 'DFS').

### Hugging Face Transformers

To use Tree of Thoughts with Hugging Face Transformers, create a custom model class that inherits from `AbstractLanguageModel` and implements the required methods using Hugging Face Transformers. Then, create an instance of the `TreeOfThoughts` class with the custom model and the desired search algorithm ('BFS' or 'DFS').

## Roadmap

Provide ready to use generate thoughts function

Provide ready to use evaluate states function



### Multi-Modality Tree of Thoughts 🌐🌳

The next big advancement for the Tree of Thoughts algorithm is to extend it to multi-modality, enabling it to handle not only text but also images, audio, and other data types. This will bring us closer to multi-modal superintelligence.

#### Actionable Steps

1. Research and identify suitable multi-modal pre-trained models that can handle various data types (e.g., text, images, audio).
2. Adapt the thought decomposition, thought generator, and state evaluator functions to handle multi-modal data.
3. Develop a method for combining different modalities in the search tree, allowing the algorithm to reason across different data types.
4. Implement and test the multi-modal Tree of Thoughts algorithm with various problems and datasets.
5. Optimize the algorithm for performance and resource usage, ensuring it scales well with large multi-modal datasets.
6. Publish the results and gather feedback from the community to further improve the multi-modal Tree of Thoughts algorithm.

Join us on this exciting journey to advance the Tree of Thoughts algorithm to multi-modality superintelligence! 🚀