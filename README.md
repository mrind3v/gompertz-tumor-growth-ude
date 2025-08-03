# Gompertz Tumor Growth Modeling with Universal Differential Equations

Universal Differential Equation (UDE) to discover the parameters of the Gompertz ODE. The project applies this scientific machine learning (SciML) technique to a tumor growth model, showcasing how to learn the underlying dynamics of a system directly from data.

The core idea is to combine a known mechanistic model (the Gompertz equation) with the flexibility of machine learning (neural networks) to learn unknown parameters that are often difficult or impossible to measure directly.

## Core Concepts

### The Mathematical Model

The project is based on a modified Gompertz Ordinary Differential Equation (ODE), a common model for describing growth processes that are initially fast but slow down as they approach a limit. This is particularly applicable to tumor growth dynamics.

The central equation is:
$$
\frac{dT}{dt} = \underbrace{r T \ln\left(\frac{K}{T}\right)}_{\text{Gompertz Growth}} - \underbrace{c(t)T}_{\text{Chemotherapy Effect}}
$$

Where:
*   `T(t)`: The size of the tumor at time `t`.
*   `r`: The intrinsic **tumor growth rate**.
*   `K`: The **carrying capacity**, or the maximum size the tumor can reach.
*   `c(t)`: The time-dependent kill rate induced by chemotherapy.

### The Universal Differential Equation (UDE) Approach

In many real-world biological systems, parameters like the growth rate (`r`) and the carrying capacity (`K`) are unknown. The goal of this project is to discover these parameters from observed data.

This is achieved by replacing `r` and `K` with two small neural networks (`Lux.jl`). The UDE solver then trains the weights of these neural networks until their output accurately reproduces the observed tumor growth data. This creates a hybrid model that combines our domain knowledge (the structure of the Gompertz equation) with the expressive power of machine learning.

## Project Workflow

The demonstration is structured as a Julia script or notebook that walks through the entire process:

1.  **Generating Synthetic Data**: We first create a "ground truth" dataset by solving the Gompertz ODE with known, fixed values for `r` and `K`. This gives us a target to train our model against.

2.  **Defining the UDE Model**: We define a new ODE function where `r` and `K` are no longer fixed numbers but are instead the outputs of two neural networks. The parameters of these networks are managed using `ComponentArrays.jl` for readability and efficiency.

3.  **Creating the Loss Function**: A loss function is defined to measure the difference between the UDE's prediction and the synthetic "ground truth" data. This function calculates the sum of squared errors and includes crucial safety checks to handle cases where the solver fails, returning `Inf` to penalize unrealistic parameters.

4.  **Training the Model**: We define an `OptimizationProblem` and use the powerful **L-BFGS optimizer** to train the UDE. The optimizer's goal is to find the weights and biases for the neural networks that minimize the loss function.

5.  **Evaluation**: After training, we plot the UDE's prediction against the ground truth data to visually assess its performance.

6.  **Extrapolation**: As the final and most important test, we use the trained model to predict the tumor's growth far beyond the time range of the training data. This tests the model's ability to generalize and demonstrates that it has learned the true underlying dynamics of the system, not just memorized the data.

## Results

The trained UDE successfully learns the underlying parameters from the data.

**Evaluation Plot:** The model's prediction (blue dashed line) almost perfectly overlays the true data (red line) it was trained on.

*(You can insert your evaluation plot here)*
``

**Extrapolation Plot:** More importantly, the model accurately predicts the tumor's trajectory for 50 days beyond the training data, demonstrating that it has learned the system's true dynamics.

*(You can insert your extrapolation plot here)*
``

## Getting Started

To run this project on your local machine, follow these steps.

### Prerequisites
*   Julia v1.8 or later. You can download it from [julialang.org](https://julialang.org/).
*   Git for cloning the repository.

### Installation & Usage

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/gompertz-tumor-growth-ude.git
    cd gompertz-tumor-growth-ude
    ```

2.  **Start Julia and activate the project environment:**
    ```julia
    # Press ']' to enter the Pkg REPL
    pkg> activate .
    pkg> instantiate
    # Press backspace to exit the Pkg REPL
    ```
    This will download and install all the necessary packages (`DifferentialEquations.jl`, `Lux.jl`, etc.) defined in the `Project.toml` file.

3.  **Run the code:**
    You can now open the Julia script or notebook and run the cells to reproduce the results.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.