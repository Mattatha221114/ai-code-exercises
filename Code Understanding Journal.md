# CODE UNDERSTANDING JOURNAL
# Part 1: Feature Exploration (Task Creation and Status Updates)
 > Files involved: cli.js, task_list.js, task.js
  * My Findings:
     I started by trying to figure out how a task actually gets created. It turns out it's a three-step process. The entry point is cli.js, which listens for the user's command in the terminal. When a user types a command to add a task, cli.js doesn't do the heavy lifting—it just passes the text to task_list.js.

     In task_list.js, the TaskList class acts like a manager. It takes that text and uses the Task class (from task.js) to stamp out a brand new task object with a unique ID and a default status of "pending". What really helped me understand this was asking the AI to explain the relationship between these three files. It showed me how they pass data down the chain like a bucket brigade.



# Part 2: Deep Dive (Task Priorities)
 > Initial Grasp:
     Before looking too closely, I assumed priorities were just a simple text tag (like "high" or "low") attached to a task, and that the display logic just printed whatever that tag was.

 > My Discoveries & AI Insights:
     After using the AI to prompt for guided questions, I dug deeper and realized it's a bit more structured. The priority isn't just a label; it dictates how the TaskList handles sorting and retrieving.

         Key Insight: 
         The Task file is only responsible for holding the priority data, but the task_list.js file is responsible for acting on it (like sorting high-priority tasks to the top).

         Clarified Misconception: 
         I initially thought the storage file would save tasks in order of priority. I was wrong. The storage.js file just saves the raw data exactly as it receives it. Sorting only happens in memory via the domain logic.



# Part 3: Data Flow Mapping (Task Completion)
 > Feature: Marking a task as complete.
  * Data Flow Map:
     1. User Input: User runs a command like complete 3 in the terminal.
     2. Routing (cli.js): The CLI captures the ID 3 and calls taskList.markComplete(3).
     3. State Change (task_list.js & task.js): The TaskList searches its memory for the task with ID 3. Once found, it updates that specific task object's status property from "pending" to "completed".
     4. Persistence (storage.js): After the state is updated in memory, TaskList calls storage.save(). The storage module converts the updated JavaScript list back into a JSON string and overwrites the old database file on the hard drive.

  * Potential Points of Failure:
     - What if the user inputs an ID that doesn't exist? (The program needs to handle "Task not found" errors gracefully).
     - What if the hard drive is full or locked? (storage.js might crash when trying to write the JSON file).



# Part 4: Presentation Outline
 * Prep notes for the 3–5 minute group share:
     >Architecture Overview: 
     I'll explain the "Separation of Concerns" pattern. We have three layers: the Interface (cli.js), the Brains/Domain (task_list.js and task.js), and the File Cabinet/Infrastructure (storage.js).

     >Key Features: 
     I'll briefly walk through how adding a task and marking it complete flows through these three layers.

     >Interesting Design Pattern: 
     The way data is isolated. The fact that storage.js has absolutely no idea what a "Task" is—it just knows how to write JSON—is a great example of decoupled code.

     >Biggest Challenge: 
     Initially, figuring out where new features (like adding a CSV export or an overdue rule) should live. I learned that you have to respect the architectural boundaries rather than just cramming code wherever it fits.