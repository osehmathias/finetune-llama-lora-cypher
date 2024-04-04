# Generate synthetic data for fine tuning LLMs to create cypher queries from natural language.

There are four notebooks in this repo:

1. Generate Questions
2. Generate Queries
3. Test Queries
4. Create Additional Questions

It creates questions, then a cypher query for each question.

Queries are tested and soreted into three files - Pass, Error, No Data. You should review the latter two piles

We then create 10 different questions for each question / query pair. Those additional questions should lead to the same query.

---

## 1. Generate Questions

This notebook introspects the Neo4j schema and creates a list of questions about the data set using the themes you have defined.

You should aim for about 20 - 30 questions per theme.

You want at least 200 questions.

## 2. Generate Queries

This notebook goes through each question from the previous step and generates a cypher query for it together with an introspection of the database.

## 3. Test Queries

This notebook tests each query on the database and sorts them into three CSVs:

1. Pass
2. Error
3. No Data

You will need to check and fix those that pass or produce no data.

## 4. Create Additional Queries

For each question / query pair, create 10 different questions that should end up at the same cypher query and add them to a CSV.

Once this has run, your model is ready to find tune.
