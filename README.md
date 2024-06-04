# financify_api

## Intro

I started this project as a way to learn the basics of REST APIs and fullstack communication in general.

## Setup

### Prerequisites

1. Linux
    - I simply don't want to worry about how this works on a Windows environment, feel free to sort through the muck on your own though
2. Python Poetry
    - Follow [these instructions](https://python-poetry.org/docs/#installing-with-the-official-installer) to install using the official Poetry installer

### Installation

1. Install using Poetry
    - From the root directory run `poetry install`
2. Run setup entry point to configure .env and .db
    `poetry run setup --init_env`
3. Run unit tests to make sure everything runs correctly
    `poetry run pytest`

## Explanation of This Project

### Motivation

- I have used Python to request resources from REST APIs at three jobs now, but I didn't fully understand what a REST API actually is.
- I have not directly used relational databases or query languages at past jobs so using sqlite was a solid introduction.
- This project (and hopefully others like it) are great ways for me to keep practicing writing code while in between jobs or in positions where I never have to write code.
- I love learning new things, and I find that having a tangible goal often helps me focus while working on projects. My goal with this project is to make an example of how a RESTful backend could operate for a simple web tool.

### Reasons Behing the Tech Stack

- Python is my strongest language so I knew the fastest way for me to learn REST would be via Python
- Django can whip up a simple web page very fast, but flask (and also flask_restful) are perfect for just crafting some endpoints 
- sqlite3 ships with Python so there's no setup
  - Note: I did have to intentionally open and close the db client for each query to avoid locking up sqlite3
- Believe it or not, despite 4 years in QA/test automation, I haven't used Pytest yet (normally I like to leave unit testing to devs while I get to build integrated system tests)
- Poetry and Tox are both awesome, and I try to use them for all Python projects

### My Learning Resources and Process

- [Roy Thomas Fielding's Explanation of REST](https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
    - Takeways:
      - REST is a set of architecture constraints that can be satisfied in a number of ways
      - Servers do not keep track of client information between requests (stateless client-server interactions)
        - This means we need to secure user data via some other method
      - Important Vocab:
        - Resource -> intended target of an http endpoint (Database record)
        - Resource Identifier -> string or code that tells the API which resource to focus on (URL)
        - Representation -> data returned by the API relevant to a resource (JSON object)
    
- [Flask Restful Minimal API Example](https://flask-restful.readthedocs.io/en/latest/quickstart.html#a-minimal-api)
    - Takeaways:
        - Can configure endpoints so part of the endpoint is used for data processing
            - Example: assets/1 explicitly references the asset with ID = 1
        - Resources can handle JSON data sent with the HTTP request
            - This means we have at least 2 methods for passing data to the API as part of a request

- [Flask Define and Access the Database](https://flask.palletsprojects.com/en/3.0.x/tutorial/database)
    - Takeaways:
        - Initialize .db files using the Python sqlite library and a .sql schema file
        - Flask apps can be built from mappings where we can set custom keys
            - Can use a key to set current mode for server (admin or basic)

- [SQLite Keywords](https://sqlite.org/lang.html)
    - Takeaways:
        - Similar to a lot of the SQL practice problems I did when prepping for interviews I knew I would flub
        - SQLite is the most barebones way to POC a database as far as I can tell
        - Maybe this is on me, but this was my least favorite subject under this project

- [OWASP Top 10 API Security Risks - 2023](https://owasp.org/API-Security/editions/2023/en/0x11-t10)
    - Takeaways:
        - Learning one or two endpoints for an API can help a person extrapolate the general endpoint structure. This can allow for brute force attemtps, so there must be some sort of restriction in place to limit API access.
        - The method I came up with for limiting access is requring an API key added to JSON data as part of a request
            - Note: This could be unsafe if using the API from a publicly accessible website, as somebody could just inspect network requests and see what API key is being passed from the browser to the server.
        - All your endpoints can be sent all HTTP verbs as requests, so you need to account for POSTs being hit against your GET-only endpoints.
        - Don't try to come up with some complex security process, just follow the [OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)

