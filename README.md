# FastAPI Automated Testing Project 🚀

# 📌 Overview

This project demonstrates API development and automated testing using FastAPI, Pytest, and GitHub Actions for continuous integration (CI).

# 📂 Project Structure

📦 fastapi-testing
 ┣ 📂 .github/workflows
 ┃ ┗ 📜 test.yml      # GitHub Actions Workflow for CI
 ┣ 📜 apiserver.py    # FastAPI server implementation
 ┣ 📜 testAutomationPytest.py  # Automated API tests using Pytest
 ┣ 📜 requirements.txt  # Project dependencies
 ┣ 📜 README.md       # Documentation
 ┗ 📜 .gitignore      # Ignored files
 
# 🛠️ Task 1: Setting Up FastAPI

1️⃣ Install FastAPI & Uvicorn

pip install fastapi uvicorn

2️⃣ Create apiserver.py

from fastapi import FastAPI

app = FastAPI()

@app.get("/add/{a}/{b}")
def add(a: int, b: int):
    return {"result": a + b}

@app.get("/subtract/{a}/{b}")
def subtract(a: int, b: int):
    return {"result": a - b}

@app.get("/multiply/{a}/{b}")
def multiply(a: int, b: int):
    return {"result": a * b}
    
3️⃣ Run the API Server

uvicorn apiserver:app --reload

# 🧪 Task 2: Writing Automated Tests for the API

Now, let's write tests for the FastAPI server using Python's requests library.

Install Requests: You will need the requests library to send HTTP requests to the server.

Install it using the following:

pip install requests

Create the Test Script (testAutomation.py): Here's a script that tests the add, subtract, and multiply endpoints:

import requests

testcases = [
    {"url": "http://localhost:8000/add/2/2", "expected": 4, "description": "Test addition of 2 and 2"},
    {"url": "http://localhost:8000/subtract/2/2", "expected": 0, "description": "Test subtraction of 2 from 2"},
    {"url": "http://localhost:8000/multiply/2/2", "expected": 4, "description": "Test multiplication of 2 and 2"}
]

def test():
    for case in testcases:
        response = requests.get(case["url"])
        result = response.json()["result"]
        assert result == case["expected"], f"Test failed: {case['description']}. Expected {case['expected']}, got {result}"
        print(f"Test passed: {case['description']}")

    print("All tests passed!")

test()
Run the Tests: Run the test script by executing:

python testAutomation.py

# 🛠️ Task 3: Enhancing the Tests with Pytest

While the requests approach is useful, let's enhance the testing process using pytest, which offers better reporting, test organization, and more features like parameterized testing.

Install Pytest: Install pytest via pip:

pip install pytest
Create the Pytest Script (testAutomationPytest.py): Modify your test script to use pytest for parameterized testing.

import pytest
import requests

testcases = [
    ("http://localhost:8000/add/2/2", 4, "Test addition of 2 and 2"),
    ("http://localhost:8000/subtract/2/2", 0, "Test subtraction of 2 from 2"),
    ("http://localhost:8000/multiply/2/2", 4, "Test multiplication of 2 and 2"),
    ("http://localhost:8000/add/-1/1", 0, "Test addition of -1 and 1"),
    ("http://localhost:8000/multiply/0/5", 0, "Test multiplication by zero"),
]

@pytest.mark.parametrize("url, expected, description", testcases)
def test_api(url, expected, description):
    response = requests.get(url)
    result = response.json()["result"]
    assert result == expected, f"{description}. Expected {expected}, got {result}"
Run the Tests with Pytest: Execute the tests using pytest:

pytest testAutomationPytest.py

# 🤖 Task 4: Integrating Test Automation with GitHub Actions

1️⃣ Create .github/workflows/test.yml

name: API Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.10"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install fastapi uvicorn pytest requests

      - name: Start FastAPI server
        run: |
          nohup uvicorn apiserver:app --host 0.0.0.0 --port 8000 &

      - name: Wait for server to start
        run: sleep 5

      - name: Run tests
        run: pytest testAutomationPytest.py
        
2️⃣ Commit & Push Changes

git add .
git commit -m "Added GitHub Actions workflow for API testing"
git push origin main

✅ Check GitHub Actions Status
Go to GitHub → Actions to verify the tests run successfully.

