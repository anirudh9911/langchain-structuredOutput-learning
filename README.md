# LangChain Structured Output

A hands-on learning project exploring different ways to extract structured data from LLMs using LangChain's `with_structured_output()` method.

## What This Covers

LangChain's `with_structured_output()` lets you force an LLM to return data in a defined schema instead of free-form text. This repo demos three schema approaches and two LLM providers.

## Project Structure

```
├── Typed_dict.py                          # Python TypedDict basics
├── pydantic_demo.py                       # Pydantic BaseModel basics
├── with_structured_output_typeddict.py    # Structured output using TypedDict
├── with_structured_output_pydantic.py     # Structured output using Pydantic
├── with_structured_output_jsonSchema.py   # Structured output using raw JSON Schema
├── with_structured_output_llama.py        # Structured output with HuggingFace (TinyLlama)
├── json_schema.json                       # Sample JSON Schema definition
└── requirements.txt                       # Python dependencies
```

## Schema Approaches

### 1. TypedDict — [`with_structured_output_typeddict.py`](with_structured_output_typeddict.py)
Uses Python's `TypedDict` with `Annotated` to attach field descriptions inline.

```python
class Review(TypedDict):
    summary: Annotated[str, "A brief summary of the review"]
    sentiment: Annotated[Literal["pos", "neg"], "Return the sentiment"]
```

### 2. Pydantic — [`with_structured_output_pydantic.py`](with_structured_output_pydantic.py)
Uses Pydantic `BaseModel` with `Field()` for richer validation and descriptions.

```python
class Review(BaseModel):
    summary: str = Field(description="A brief summary of the review")
    sentiment: Literal["pos", "neg"] = Field(description="Return the sentiment")
```

### 3. JSON Schema — [`with_structured_output_jsonSchema.py`](with_structured_output_jsonSchema.py)
Passes a raw JSON Schema dict directly — useful when you want provider-agnostic schema definitions.

```python
structured_model = model.with_structured_output(json_schema)
```

## LLM Providers

| File | Provider | Model |
|------|----------|-------|
| `with_structured_output_pydantic.py` | OpenAI | `gpt-4o` (default) |
| `with_structured_output_typeddict.py` | OpenAI | `gpt-4o` (default) |
| `with_structured_output_jsonSchema.py` | OpenAI | `gpt-4o` (default) |
| `with_structured_output_llama.py` | HuggingFace | `TinyLlama-1.1B-Chat` |

## Setup

**1. Clone the repo and create a virtual environment:**
```bash
git clone https://github.com/YOUR_USERNAME/langchain-structured-output.git
cd langchain-structured-output
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
```

**2. Install dependencies:**
```bash
pip install -r requirements.txt
```

**3. Set up environment variables:**
```bash
cp .env.example .env
```
Edit `.env` and add your API keys:
```
OPENAI_API_KEY=your_openai_key_here
HUGGINGFACEHUB_API_TOKEN=your_huggingface_token_here
```

## Running the Examples

```bash
# TypedDict schema with OpenAI
python with_structured_output_typeddict.py

# Pydantic schema with OpenAI
python with_structured_output_pydantic.py

# Raw JSON Schema with OpenAI
python with_structured_output_jsonSchema.py

# Pydantic schema with HuggingFace TinyLlama
python with_structured_output_llama.py
```

## Key Concepts

- **`with_structured_output()`** — LangChain method that wraps any chat model to return structured data matching your schema
- **TypedDict** — Lightweight, no runtime validation, good for simple schemas
- **Pydantic BaseModel** — Full validation, type coercion, and rich field metadata
- **JSON Schema** — Raw dict format, provider-agnostic, no Python class needed

## Prerequisites

- Python 3.10+
- OpenAI API key (for OpenAI examples)
- HuggingFace API token (for the LLaMA example)
