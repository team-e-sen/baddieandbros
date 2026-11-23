# Test Plan: Inventory Management System

1. Introduction

This document outlines the testing strategy for the Python-based Inventory Management System. It ensures that all functional requirements are met before a baseline is released.

1. Test Environment
   Language: Python 3.x
   Testing Framework: unittest (built-in Python)
   Data Storage: Local JSON file (inventory.json)
   CI/CD: GitHub Actions (automated test runner)
2. Test Cases
   TC-1: Add Item (Happy Path)
   Action: Add a new item with name "Laptop", quantity 5, and price 1000.
   Expected Result: Item is saved to the JSON file and appears in the inventory list.
   TC-2: Add Item (Negative Quantity)
   Action: Attempt to add an item with a negative quantity (e.g., -5).
   Expected Result: The system rejects the input and displays an error message.
   TC-3: List Items
   Action: Run the list command when there are items in the inventory.
   Expected Result: The system displays all items, their quantities, and prices in a formatted table.
   TC-4: Update Quantity (Bug Fix Verification)
   Action: Update the quantity of an existing item.
   Expected Result: The JSON file is updated correctly, and the new quantity is reflected in the list command.
3. Defect Management

Any bugs found during testing (e.g., a date parsing bug or a JSON read error) will be logged in Jira as a Bug issue type, assigned to a developer, and linked to the relevant Change Request.
