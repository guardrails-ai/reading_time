## Overview

| Developed by | Guardrails AI |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Format |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description

This validator ensures that any LLM generated text is less than some maximum expected reading time. The reading time estimation is done at 200 wpm.

## Installation

```bash
$ guardrails hub install hub://guardrails/reading_time
```

## Usage Examples

### Validating string output via Python

In this example, we’ll use the validator to validate that an LLM description is under 2 minutes of reading time.

```python
# Import Guard and Validator
from guardrails.hub import ReadingTime
from guardrails import Guard

# Initialize Validator
val = ReadingTime(
    reading_time=2,
    on_fail="fix"
)

# Setup Guard
guard = Guard.from_string(
    validators=[val, ...],
)

guard.parse("As short string")  # Validator passes
```

### Validating JSON output via Python

In this example, we’ll generate JSON about a pet, and validate that one of the JSON fields has reading time of under 2 minutes.

```python
# Import Guard and Validator
from pydantic import BaseModel
from guardrails.hub import ValidChoices
from guardrails import Guard

val = ReadingTime(
    reading_time=2,
    on_fail="fix"
)

# Create Pydantic BaseModel
class PetInfo(BaseModel):
    pet_name: str
    pet_history: str = Field(validators=[val])

# Create a Guard to check for valid Pydantic output
guard = Guard.from_pydantic(output_class=PetInfo)

# Run LLM output generating JSON through guard
guard.parse("""
{
    "pet_name": "Caesar",
    "pet_history": "This pet has been a very good boy for many years."
}
""")
```

## API Reference

`__init__`

- `reading_time`: The maximum reading time in minutes.
- `on_fail`: The policy to enact when a validator fails.
