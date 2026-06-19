# 1. Algorithm Breakdown
  * Algorithm Selected: Algorithm 3 - JavaScript Implementation.

  * What it does: 
     This algorithm acts as a traffic controller for offline/online task management. It takes two sets of tasks (a local dictionary and a remote dictionary) and merges them into a single source of truth, keeping track of exactly which tasks need to be uploaded, downloaded, or updated to keep both systems perfectly mirrored.

  * Step-by-Step Execution:
     1. Gather IDs: 
         It first creates a mathematical "Set" of all unique Task IDs from both the local and remote sources.

     2. Triage: 
        - It loops through every ID and categorizes it into one of three buckets:

             > Missing Remote: 
             The task exists locally but not remotely (Action: Create on remote).

             > Missing Local: 
             The task exists remotely but not locally (Action: Create on local).
             
             > Conflict: 
             The task exists in both places (Action: Trigger resolveTaskConflict).

     3. Conflict Resolution (resolveTaskConflict):
         > Base copy: 
             It creates a safe clone of the local task to avoid accidentally mutating the original data.

         > Time-based merge: 
             It compares the updatedAt timestamps. The newest version overwrites the title, description, priority, and due date.

         > Status Override: 
             If either side marks the task as DONE, that status wins, regardless of the timestamp.

         > Tag Union: 
             It combines the tags from both sides, ensuring no tags are lost (using a Set to prevent duplicates).

         > Final Flagging: 
             It checks if the merged task is different from the original local or remote tasks and flags them for an update if necessary.



# 2. Insights and Learning Points
 > Defensive Copying: 
     The algorithm uses const mergedTask = {...localTask}. This is an excellent functional programming practice. By mutating a copy rather than the original object, we avoid unintended side effects elsewhere in the application.

 > Granular Conflict Resolution: 
     The algorithm doesn't just say "the newest task wins completely." It evaluates specific fields differently. For example, it intentionally prioritizes a "DONE" status over a newer "PENDING" status. This is a crucial business rule disguised as code.

 > The "Deleted Task" Blindspot: 
     A key insight from reading this logic is that it does not currently handle deleted tasks. If a task is deleted locally, it disappears from the localTasks map. During the merge, the algorithm will see it in remoteTasks but not localTasks, and assume it needs to be recreated locally (Case 2).




# 3. Reflection Questions
 > How did the AI’s explanation change your understanding of the algorithm?
     Initially, I thought merging was a simple "replace old with new" operation. Breaking it down made me realize that two-way syncs require highly specific "buckets" (what to create vs. what to update) and that individual properties within an object (like tags vs. status) might need entirely different resolution rules.

 > What aspects were still difficult to understand after AI explanation?
     The exact mechanics of checking array equality for the tags (arraysEqual helper function) took a moment to grasp. Because JavaScript arrays are passed by reference, you cannot simply do array1 === array2. The algorithm has to explicitly sort and compare every single value to know if the tags actually changed.

 > How would you explain this algorithm to another junior developer?
     I would use the analogy of two people editing a shared document while offline. When they reconnect, the system needs to look at their changes. If one person wrote a new paragraph, we add it for the other. If both edited the same sentence, we look at the clock and take the most recent edit. But, if one person checked off a checklist item as "DONE", we keep it as "DONE" for both, because we don't want to accidentally un-complete someone's hard work.

 > Did you test this understanding against AI?
     Yes. I mentally tested the edge case of "What happens if local is updated today with a new title, but remote was marked DONE yesterday?" Based on the code, the title will update to the local version (timestamp wins), but the status will remain DONE (status logic overrides timestamp).


 > How might you improve the algorithm based on your understanding?
   > Tombstone Handling for Deletions: 
         I would introduce "tombstones" (a flag like isDeleted: true). Instead of removing a task from the dictionary when the user deletes it, we mark it as deleted instead. This allows the merge algorithm to see the deletion, compare timestamps, and successfully delete it from the remote server rather than accidentally reviving it.

   > Deep Copying: 
         The line const mergedTask = {...localTask}; only performs a shallow copy. Since tags are an array (a reference type), modifying the tags array directly could still cause side effects. Using structured cloning or a deep copy utility would make this safer.