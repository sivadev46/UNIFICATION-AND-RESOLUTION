# UNIFICATION AND RESOLUTION ALGORITHM

## Experiment 2(c): Implementation of Unification and Resolution Algorithm

### Aim

To implement unification and resolution algorithm using Python.

### Scenario

In an AI-based expert system for automated reasoning, the system needs to resolve queries by unifying logical predicates and applying resolution inference.

For example, given the knowledge base:

* Rule 1: If John is a human, then John is a mortal.
* Fact 1: Human(John).
* Query: Is John mortal?

### Procedure

1. Define the unification function (unify).
2. If both terms are identical, return the current substitution.
3. If one term is a variable, unify it with the other term.
4. If both terms are compound expressions, unify their corresponding parts recursively.
5. Otherwise, return None if unification fails.
6. Define the resolution function to process the knowledge base.
7. Try to unify the query with the conclusion of each rule or fact.
8. Recursively prove the conditions of the matching rule.
9. Print whether the query is resolved.

### Python Program

```python
# Function to check if two terms can be unified

def unify(x, y, theta=None):
    if theta is None:
        theta = {}

    if x == y:
        return theta

    if isinstance(x, str) and x.islower():
        if x in theta:
            return unify(theta[x], y, theta)
        theta[x] = y
        return theta

    if isinstance(y, str) and y.islower():
        if y in theta:
            return unify(x, theta[y], theta)
        theta[y] = x
        return theta

    if isinstance(x, list) and isinstance(y, list):
        if len(x) != len(y):
            return None

        for a, b in zip(x, y):
            theta = unify(a, b, theta)
            if theta is None:
                return None

        return theta

    return None


# Knowledge Base: Facts and Rules

facts = [
    ["Human", "John"]
]

rules = [
    (["Mortal", "x"], [["Human", "x"]])
]


# Function to apply substitution

def substitute(term, theta):
    if isinstance(term, list):
        return [substitute(item, theta) for item in term]

    while isinstance(term, str) and term in theta:
        term = theta[term]

    return term


# Resolution function

def resolution(query):
    # Check facts
    for fact in facts:
        if unify(query, fact, {}) is not None:
            return True

    # Check rules
    for conclusion, conditions in rules:
        theta = unify(query, conclusion, {})

        if theta is not None:
            resolved = True

            for condition in conditions:
                new_query = substitute(condition, theta)

                if not resolution(new_query):
                    resolved = False
                    break

            if resolved:
                return True

    return False


# Query: Is John Mortal?

query = ["Mortal", "John"]

# Apply resolution

if resolution(query):
    print("Query is resolved: John is Mortal")
else:
    print("Query could not be resolved")
```

### Output

```text
Query is resolved: John is Mortal
```

### Result

Thus, the Unification and Resolution algorithm was successfully implemented using Python to prove that John is mortal based on the given knowledge base.
