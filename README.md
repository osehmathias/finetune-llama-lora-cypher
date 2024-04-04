# Fine-tune Llama 2 7B & Lora with Synthetic Cypher Data

This project creates synthetic cypher data and fine tunes a model to ask questions of a Neo4j database.

## Table of Contents

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Authentication](#running-the-project)
  - [Running the Project](#running-the-project)
- [Methodology](#methodology)
  - [Generating Synthetic Data](#generating-synthetic-data)
  - [Fine-tuning the Models](#fine-tuning-the-models)
- [Testing Results](#testing-results)

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- Python 3.8 or higher
- Jupyter Notebook
- Virtualenv (optional but recommended for creating isolated Python environments)

### Installation

1. Clone the repository:
   ```sh
   git clone git@github.com:osehmathias/finetune-llama-lora-cypher.git
   cd finetune-llama-lora-cypher
   ```
2. Create and activate a virtual environment (optional):
   ```sh
   python3 -m venv venv
   source venv/bin/activate
   ```
3. Install the required packages:
   ```sh
   pip install -r requirements.txt
   ```

### Authentication

Before running the project, you must authenticate with the necessary services: Neo4j database and Predibase. Please follow the steps below to ensure you're authenticated correctly.

#### Neo4j Database

1. **Environment Variables**: The project uses environment variables to securely manage database credentials. You need to create a `.env` file at the root of the project with the following content:

   ```plaintext
   NEO4J_URI=<your-neo4j-database-uri>
   NEO4J_USERNAME=<your-neo4j-username>
   NEO4J_PASSWORD=<your-neo4j-password>
   ```

   Replace `<your-neo4j-database-uri>`, `<your-neo4j-username>`, and `<your-neo4j-password>` with your Neo4j database URI, username, and password, respectively.

2. **Load Environment Variables**: Make sure that the environment variables are loaded into your session before running the project. If you're using a virtual environment, this step might be automated when you activate it.

#### Predibase

1. **API Key**: To use Predibase services, you must first obtain an API key from your Predibase account.

2. **Login via CLI**: Open your terminal and use the following command to login to Predibase:

   ```sh
   pbase login
   ```

   When prompted, enter your Predibase API key. This will authenticate your session, allowing you to interact with Predibase services through the CLI.

#### OpenAI

1. **Environment Variable for OpenAI**: Alongside the Neo4j credentials in your `.env` file, add your OpenAI API key:

   ```plaintext
   OPENAI_API_KEY=<your-openai-api-key>
   ```

   Replace `<your-openai-api-key>` with your actual OpenAI API key.

Ensure all sensitive information, such as API keys and database credentials, is stored securely and never shared publicly.

### Running the Project

1. Start by generating questions from the schema:

   ```sh
   jupyter notebook generate_synthetic_data/1_generate_questions.ipynb
   ```

   Follow the instructions in the notebook to generate synthetic questions.

2. Continue with generating queries from the questions:

   ```sh
   jupyter notebook generate_synthetic_data/2_generate_queries.ipynb
   ```

3. To test the queries against the database:

   ```sh
   jupyter notebook generate_synthetic_data/3_test_queries.ipynb
   ```

   This sorts the queries into - Pass, No Data, Error. Review the latter two.

4. Test the queries and additional questions:

   ```sh
   jupyter notebook generate_synthetic_data/4_generate_additional_questions.ipynb
   ```

5. Fine tune Llama 2 7b with the dataset
   ```sh
   jupyter notebook fine_tune.ipynb
   ```

## Methodology

### Generating Synthetic Data

1. **Question Generation Using GPT-4**: Use GPT-4 to generate sets of 20 questions based on predefined themes related to the schema, ensuring thematic relevance and diversity.

2. **Cypher Query Generation**: For each generated question, utilize GPT-4 to create a corresponding Cypher query, mapping natural language inputs to executable database queries.

3. **Query Validation with Neo4j**: Execute each Cypher query against the Neo4j database using the Neo4j driver, sorting the results into categories: pass (successful execution), error, or no data. Some human intervention is needed for queries with no error or no data.

4. **Additional Question Generation**: Employ GPT-4 to generate 10 additional questions for each theme, aimed at leading to the same Cypher queries. This enriches the dataset with varied phrasing and structural approaches while maintaining the original query intent.

### Fine-tuning the Models

#### Model Fine-Tuning Details

For the purpose of this project, we fine-tuned two models: Llama 7B and CodeLlama 13B. Despite the superior performance of CodeLlama 13B in generating queries in the correct format with minimal need for additional prompting, it is not available for deployment on the Developer tier. Therefore, we proceeded with deploying Llama 7B for this notebook.

#### Training Parameters

- **Epochs**: 3
- **Learning Rate**: 0.0002

## Training Loss

![alt text](/misc/train-image.png "Title")
