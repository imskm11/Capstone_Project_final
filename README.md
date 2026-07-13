# Part 4 – LLM-Powered Feature

## Student Information

**Track Chosen:** Track C – Model Prediction Explanation Pipeline

---

# Project Overview

In this project, a Large Language Model (LLM) is integrated with the best machine learning model developed in Part 3. The trained model predicts the class and probability for a given feature vector, while the LLM generates a structured JSON explanation of the prediction.

The implementation uses the OpenRouter API through the Python requests library and validates all generated JSON responses using the jsonschema package.

---

# Objective

The objective of this project is to:

- Load the best trained model from Part 3.
- Predict class labels and probabilities.
- Generate structured prediction explanations using an LLM.
- Validate all generated JSON outputs.
- Apply safety guardrails before making API calls.
- Compare deterministic and creative LLM outputs using different temperatures.

---

# Technologies Used

- Python
- Google Colab
- Pandas
- NumPy
- Scikit-learn
- Joblib
- Requests
- JSON
- JsonSchema
- OpenRouter API
- GPT-4o Mini

---

# Model Used

The prediction model loaded in this project is:

```
best_model.pkl
```

The model is loaded using:

```python
joblib.load("best_model.pkl")
```

---

# API Connection

The OpenRouter API was used for accessing the language model.

The API key is stored securely using an environment variable.

```python
os.environ["LLM_API_KEY"]
```

The API key is never hardcoded in the notebook.

---

# Reusable LLM Function

A reusable function named

```
call_llm()
```

was implemented.

The function:

- creates the JSON payload
- sends HTTP POST request
- handles API errors
- returns the generated response

The function was successfully tested using the prompt:

```
Reply with only the word: hello
```

Output:

```
hello
```

---

# System Prompt

```
You are an expert Machine Learning Prediction Explanation Assistant.

Your task is to explain machine learning predictions clearly and professionally.

Rules:

1. Explain the prediction in simple language.
2. Mention the predicted class.
3. Mention the confidence score.
4. Explain the important features.
5. Return only valid JSON.
6. Do not generate false information.
```

---

# User Prompt Template

```
Predicted Class:
{predicted_class}

Confidence Score:
{confidence_score}

Input Features:
{feature_values}

Return only valid JSON in the required format.
```

---

# Temperature Choice

Temperature = **0.0** was selected because structured JSON generation requires deterministic and consistent outputs.

A low temperature always selects the highest probability token, making the output reproducible and suitable for production systems.

---

# Temperature Comparison

| Input | Output (Temp = 0.0) | Output (Temp = 0.7) | Key Difference |
|--------|---------------------|---------------------|----------------|
| Sample 1 | Consistent JSON | More descriptive JSON | Slight wording variation |
| Sample 2 | Consistent JSON | More creative explanation | Additional descriptive text |
| Sample 3 | Consistent JSON | More natural language | Less deterministic |

### Observation

Temperature **0.0** generated deterministic and repeatable responses, making it suitable for structured machine learning explanations. Temperature **0.7** produced more varied wording and creative explanations while preserving the overall meaning.

---

# JSON Schema Validation

Every LLM response is:

- Parsed using

```
json.loads()
```

- Validated using

```
jsonschema.validate()
```

Required fields:

- prediction
- confidence
- summary
- important_features
- recommendation

If validation fails, a fallback dictionary is returned.

---

# PII Guardrail

Before every LLM request, the user input is checked for Personally Identifiable Information (PII).

The guardrail detects:

- Email addresses
- Phone numbers

If PII is detected:

- API request is blocked.
- The LLM is not called.

---

# Guardrail Demonstration

| Test Input | Result |
|------------|---------|
| Email present | BLOCK |
| Clean input | PASS |

---

# End-to-End Demonstration

| Input | LLM Output | Valid JSON | Pass/Block |
|--------|------------|------------|------------|
| Sample 1 | JSON Explanation Generated | PASS | PASS |
| Sample 2 | JSON Explanation Generated | PASS | PASS |
| Sample 3 | JSON Explanation Generated | PASS | PASS |

---

# Prediction Pipeline

The complete prediction workflow is:

```
Feature Vector
        ↓
Best ML Model
        ↓
Prediction
        ↓
Prediction Probability
        ↓
Prompt Construction
        ↓
LLM API
        ↓
JSON Response
        ↓
Schema Validation
        ↓
PII Guardrail
        ↓
Final Structured Explanation
```

---

# Project Structure

```
Part4/

│── Part4_LLM_Feature.ipynb
│── best_model.pkl
│── encoded_features.csv
│── X_train.csv
│── X_test.csv
│── y_train.csv
│── y_test.csv
│── README.md
```

---

# Conclusion

This project successfully combines a machine learning model with a Large Language Model to produce structured prediction explanations.

The implementation includes:

- Secure API integration
- Prompt engineering
- Structured JSON output
- JSON schema validation
- PII protection
- Temperature comparison
- End-to-end prediction pipeline

The complete notebook executes successfully from start to finish and satisfies the Track C requirements.
