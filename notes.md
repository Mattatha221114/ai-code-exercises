
# EXCERCISE PART 1: UNDERSTANDING PROJECT STRUCTURE


> 1 EXPLORING THE CODEBASE
  * Navigated through the JavaScript Task Manager project folder.
  * Reviewed package.json to understand the project dependencies and configuration.
  * Identified the main files:
     - `cli.js`
     - `storage.js`
     - `task.js`
     - `task_list.js`



> 2 FORM INITIAL UNDERSTANDING:
  * Codebase orginisation
     - The project uses a modular structure with the different responsibilities separated into different files.
     - This makes the code easier to read, maintain, and update.

  * Technologies and Frameworks
     - JavaScript `(Node.js)`: Main language and runtime.
     - Commander: Handles command-line inputs and commands.
     - UUID: Generates unique IDs for tasks.
     - Jest: Used for automated testing.

  * Main Components
     - `cli.js` Manages user interaction and command execution.
     - `storage.js` Saves and loads task data.
     - `task.js` & `task_list.js` Contain the core task management logic, including creating, updating, and deleting tasks.



> 3 APPLYING THE PROJECT STRUCTURE PROMPT
  * Prompt Used 
     - Understanding the project structure and technology stack

  * Initial Understanding
     - Believed that the project was a command-line to-do list application.
     - Question Asked: "Does `storage.js` save data to a database or local file?"

  * AI Findings
     - Confirmed that it is a modular CLI application
     - Clarified that the `storage.js` most likely stores tasks in local JSON file using `Node.js` file system functionality, and not a database.



> 4 FINDINGS
  * Misconseptions
     - Initially expected a requirement.txt file
     - Learned that Node.js projects use package.json instead of Python's dependency file.

  * EntrybPoint & Architecture
     - Entry Point: `cli.js`
     - Architecture: Separation of Concerns
         - User Input → Business Logic → Data Storage

  * Key Responsibilities
     - `cli.js` – Parses terminal commands and triggers actions.
     - `task_list.js` – Manages the collection of tasks.
     - `task.js` – Defines individual tasks, including ID, description, and status.
     - `storage.js` – Persists task data locally so it remains available between sessions.





# Exercise Part 2: Finding Feature Implementation (CSV Export)

> 1. Initial Search
  * Reviewed `cli.js` to understand how commands are registered.
  * Examined `storage.js` to see how data is saved and loaded.
  * Checked `task_list.js` to understand how tasks are stored and retrieved.



> 2. Form Hypothesis
  * Add a new `export` command in `cli.js`.
  * Retrieve the current task list from `task_list.js`.
  * Handle CSV formatting and file creation in `storage.js` to keep responsibilities separated.



> 3. Apply the Feature Location Prompt

  * Prompt Used
     - "I need to add a feature that exports the current to-do list to a CSV file. Based on the current architecture, which files should I modify and where should the core logic go?"

  * AI Findings
     - Confirmed that `cli.js` should handle the new command.
     - Recommended placing CSV export logic in `storage.js`.
     - Suggested using Node.js file system functions and simple string formatting instead of an external CSV library.



> 4. Document Findings
 * Misconceptions
     - Initially thought CSV conversion belonged in `task_list.js`.
     - Realized file formatting should be handled by `storage.js` to maintain separation of concerns.

 * Entry Point
     - `cli.js` is the main entry point for the export feature.

 * Implementation Plan
     - Step 1:** Add an `export` command in `cli.js`.
     - Step 2:** Retrieve all tasks from `task_list.js`.
     - Step 3:** Implement an `exportToCSV()` function in `storage.js` to format the data and save it as a `.csv` file.





# Exercise Part 3: Understanding the Domain Model

> 1. Extract the Domain Model
 * Reviewed `task.js` and `task_list.js` to identify the main entities.
 * Examined task properties such as ID, description, and status.
 * Analyzed methods to understand how tasks are created and managed.


>2. Form Initial Understanding
 * Core Entities
     - Task – Represents a single to-do item.
     - TaskList – Manages a collection of tasks.

 * Task Properties
     - Unique ID (UUID)
     - Description
     - Completion status

 * Relationship
     - One TaskList contains multiple Task objects.


>3. Apply the Domain Model Prompt
 * Prompt Used
     - "Explain the domain model of this application based on the codebase. What are the core business entities, their properties, and how do they interact?"

 * AI Findings
     - Confirmed that Task and TaskList are the main domain entities.
     - Verified that there are no additional entities such as User or Category in the current application.


> 4. Document Findings
 * Misconceptions
     - Initially considered `storage.js` part of the domain model.
     - Learned that it is an infrastructure component responsible for data persistence, not business logic.


 * Key Relationships
     - A TaskList contains many Task objects.
     - Each Task is managed through the TaskList.

 * Business Rules
     - Every task has a unique UUID.
     - Each task includes a description and a completion status.
     - Tasks can be added, updated, completed, or removed through the TaskList.




