---
categories:
  - Software Development
  - Coding
tags:
  - python
  - requests
  - REST API
---
## Leveraging Python for REST API Interactions: From argparse to typer
In previous blog posts, we explored essential aspects of modern development such as Continuous Integration and Continuous Deployment and Modern Software Development with a Scrum-Focused Approach. 

Today, we'll dive into the practicality of Python for interacting with RESTful APIs, specifically leveraging the **requests** library.
The integration of REST APIs is a common theme in today's interconnected world and Python, with its user-friendly ecosystem, makes it simple and effective.

We'll start by demonstrating how to use **requests** with a CLI using **argparse**, then introduce **typer**, a newer library that simplifies the creation of CLI programs.

### Interacting with REST APIs using requests

Here's a brief snippet that fetches users from a specific endpoint:

```python
import requests

response = requests.get("https://dummyjson.com/users")
data = response.json()

for user in data:
    print(user['name'])
```

### Crafting CLI Tools with argparse
The **argparse** library lets us build powerful command-line interfaces. Below is an example script:

```python
import argparse
import requests

def get_users(endpoint):
    response = requests.get(endpoint)
    json_response = response.json()
    for user in json_response.get('users', []):
        print(user['firstName'])

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Retrieve users from API.")
    parser.add_argument('--endpoint', type=str, default="https://dummyjson.com/users", help="API endpoint to fetch users")

    args = parser.parse_args()
    get_users(args.endpoint)
```

Further details on **argparse** can be found in the [official documentation](https://docs.python.org/3/library/argparse.html).

### Modernizing CLI with typer
Let's transition to using **typer**, a modern and intuitive library for crafting CLI tools.

**Typer** stands out for its ability to effortlessly transform functions into CLI commands using decorators, leveraging type annotations and docstrings to create intuitive and informative interfaces:
```python
import typer
import requests

app = typer.Typer()

@app.command()
def get_users(endpoint: str = "https://dummyjson.com/users"):
    """
    Fetch users from a specified API endpoint.
    """
    response = requests.get(endpoint)
    json_response = response.json()

    for user in json_response.get('users', []):
        print(user['firstName'])

if __name__ == "__main__":
    app()
```

Invoking the **typer** based script with the *--help* parameter generates the following output:

```bash
Usage: script.py [OPTIONS] COMMAND [ARGS]...

Options:
  --help  Show this message and exit.

Commands:
  get-users  Fetch users from a specified API endpoint.
```

For an extensive overview of typer, consult the [official documentation](https://typer.tiangolo.com/).

### How to call the script:
The overall interface to the outside is almost the same whereas the code in typer is greatly simplified especially for bigger CLI tools.

Using **argparse**:
```bash
python3 script_argparse.py --endpoint "https://dummyjson.com/users"
```

Using **typer**:
```bash
python3 script_typer.py get-users --endpoint "https://dummyjson.com/users"
```

### Conclusion
By leveraging Python's **requests** library, developers can efficiently interact with REST APIs. With **argparse** and **typer**, this functionality can be extended into flexible CLI tools. While **argparse** has long been a staple for CLI creation, **typer** introduces an even more user-friendly approach.

This post complements our series on modern development practices, adding another layer to the tools and techniques that empower today's developers.
Whether integrating continuous deployment, mastering git branching, or building CLI tools, understanding these principles is key to thriving in the ever-evolving landscape of software development.
