# Regression Model Advisor

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/gist/despinosam2/8db549ee2f845f0424c1908f1e11eea3/regression_model_selector.ipynb)

This is an interactive tool built using `ipywidgets` to help you select a suitable regression model based on characteristics of your dataset and project requirements.

## How it Works

The tool presents a series of questions about your data (linearity, dataset size, need for interpretability, etc.). Based on your input (using sliders from 0 to 10), it calculates a compatibility score for several common regression models and suggests the top models for your use case.

## How to Run This Interactive Tool

You can run this notebook in Google Colab (recommended) or in a local Jupyter environment.

### Option 1: Run in Google Colab (Easiest)

1.  Click on the "Open in Colab" badge at the top of this README.
2.  The notebook will open in Google Colab.
3.  Go to the menu and select `Runtime > Run all`.
4.  Scroll down to see the interactive sliders. Adjust the sliders to match your project needs, and the model suggestions will update below the sliders.

### Option 2: Run Locally using Jupyter Notebook or JupyterLab

1.  **Make sure you have Python installed.** If not, download it from [python.org](https://www.python.org/).
2.  **Clone this repository** to your computer. Open your terminal or command prompt and run:
    ```bash
    git clone [Link to your repository, e.g., [https://github.com/YourUsername/regression-model-advisor.git](https://github.com/YourUsername/regression-model-advisor.git)]
    ```
3.  **Navigate to the project directory:**
    ```bash
    cd regression-model-advisor # or whatever you named the folder
    ```
4.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    # On Windows:
    # .\venv\Scripts\activate
    # On macOS/Linux:
    # source venv/bin/activate
    ```
5.  **Install the required libraries:**
    ```bash
    pip install -r requirements.txt
    ```
6.  **Install JupyterLab or Jupyter Notebook:**
    ```bash
    pip install jupyterlab # or pip install notebook
    ```
7.  **Start the Jupyter server:**
    ```bash
    jupyter lab # or jupyter notebook
    ```
8.  Your web browser will open with the Jupyter interface. Navigate to the project folder and click on the `regression_model_advisor.ipynb` file (or the name you saved it as).
9.  Run the cells in the notebook to see the interactive widget.

# Explanation of the Code

This Python code, designed to run in a Jupyter Notebook environment, provides an interactive tool to help users select an appropriate regression model for their machine learning tasks. It works by asking a series of questions about the data and project requirements and then suggesting models based on how well their inherent characteristics align with the user's input.

**Here's a breakdown of the code:**

1.  **Import Libraries:**
    * `ipywidgets as widgets`: This library provides interactive HTML widgets for Jupyter notebooks, allowing users to interact with the code through sliders and other controls.
    * `from IPython.display import display, Markdown`: These functions are used to display the widgets and formatted text (using Markdown) within the Jupyter Notebook.

2.  **Define Questions:**
    * The `questions` list contains a series of questions that are crucial when choosing a regression model. These questions cover aspects like the linearity of the data, the need for interpretability, the size of the dataset, the presence of missing values, and deployment considerations.

3.  **Define Models and Their Profiles:**
    * The `models` dictionary stores information about various common regression models.
    * Each model is associated with a list of 10 integer scores (ranging from 0 to 10). Each score corresponds to one of the questions defined earlier.
    * A higher score for a particular question indicates that the model is well-suited for that characteristic. For example, "Linear Regression" scores a 10 for "Is the relationship between variables likely linear?" and "Do I need interpretability?", reflecting its strengths in these areas. Conversely, it scores low on handling nonlinearity.

4.  **Create Sliders:**
    * A list of `widgets.IntSlider` objects is created. Each slider corresponds to one of the questions.
    * The sliders allow the user to input a value between 0 and 10 for each question, indicating the importance or likelihood of that characteristic in their specific project.
    * `continuous_update=False` ensures that the `evaluate_model` function is only called when the user releases the slider, improving performance.

5.  **`evaluate_model` Function:**
    * This function is called whenever the user interacts with the sliders.
    * It takes the current values of all the sliders as input (`**kwargs`).
    * `input_scores = list(kwargs.values())`: Extracts the slider values into a list representing the user's input for each question.
    * `results = {}`: Initializes an empty dictionary to store the similarity scores for each model.
    * The code iterates through each `model` and its corresponding `profile` in the `models` dictionary.
    * `score = sum(min(i, p) for i, p in zip(input_scores, profile))`: This is the core logic for calculating a "similarity score" for each model. For each question, it takes the minimum of the user's input score (`i`) and the model's profile score (`p`). The sum of these minimums across all questions gives an overall score for how well the model's strengths align with the user's data characteristics. A higher score indicates a better potential fit.
    * `ranked = sorted(results.items(), key=lambda x: x[1], reverse=True)`: Sorts the models based on their calculated scores in descending order.
    * The function then uses `display(Markdown(...))` to present the top 3 suggested models and the full ranking of all models along with their scores in a user-friendly format within the Jupyter Notebook.

6.  **Create Interactive UI:**
    * `interactive_ui = widgets.interactive(evaluate_model, **{f"Q{i+1}": sliders[i] for i in range(len(questions))})`: This creates the interactive user interface. It links the `evaluate_model` function to the sliders. The `**{f"Q{i+1}": sliders[i] for i in range(len(questions))}` part dynamically creates keyword arguments for the `evaluate_model` function, where each argument name corresponds to a question number (e.g., `Q1`, `Q2`, etc.) and its value is linked to the corresponding slider.

7.  **Display Instructions and UI:**
    * `display(Markdown(...))` is used to provide a title and instructions to the user on how to use the tool.
    * The questions are displayed along with their corresponding sliders.
    * Finally, `display(interactive_ui)` renders the interactive sliders in the Jupyter Notebook, allowing the user to begin the model selection process.

**To use this code:**

1.  Save it as a `.py` file (e.g., `model_selector.py`) or directly run it in a Jupyter Notebook cell.
2.  Execute the cell. You will see the title, instructions, the list of questions with interactive sliders, and initially, the top suggested models based on the default slider values (all set to 5).
3.  Adjust the sliders according to your project's specific needs and data characteristics. As you release the sliders, the "Top Suggested Models" and "Full Scores" sections will update dynamically, providing you with model recommendations.

This tool offers a simple yet intuitive way to get a starting point for choosing a regression model by considering various important factors. Remember that this is a guideline, and further experimentation and evaluation are always necessary to determine the absolute best model for a given problem.

