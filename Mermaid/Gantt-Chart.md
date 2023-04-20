# Gantt Chart
- [Matt Eland's Gantt Chart example](https://newdevsguide.com/2023/04/14/mermaid-gantt-chart/)

> Gantt charts are **project management charts** named after their creator, Henry Gantt in the early 1900's.
> used to document **key dates** and **phases** in projects by rendering boxes starting at a task's start date and ending at that task's end date.
> Helpfull for finding the __critical path__
> To a large degree, Agile has replaced Gantt Charts

### Format
```
task name : start-date, end-date
```

### custom statuses

- **done** for completed tasks
- **active** for tasks currently in progress
- **crit** for tasks on the critical path
- **milestone** for milestones (see next section)

**Example**
```
Create code for Gantt chart     :done, crit,   2023-04-11, 2d
```

### Sections and Milestones

### Relative Scheduling of Tasks
- simulating a predecessor relationship.
- To help differentiate between valid statuses such as **done** and **active**, I like to use **uppercase identifiers** like **OUTLINE**.

### Compact Mode and Date Line
- a red "today line" at the current date 
- `displayMode` to `compact`

# Kitchen Sink Example
```mermaid
gantt
    title Preparing Polyglot Notebooks Talk for Stir Trek 2023
    section Proposal and Evaluation
        Submit Abstract         :done,                2023-01-15, 2023-02-18
        Session Evaluation      :done, EVAL           2023-02-18, 2023-03-05
        Talk Accepted           :milestone, done,     after EVAL
    section Talk Preparation
        Research & Outlining    :done, OUTLINE,       2023-03-12, 9d
        Create Mermaid Examples :done, MER_EXAMPLE,   after OUTLINE, 5d
        Write Mermaid Articles  :active, MER_ART,     after MER_EXAMPLE, 7d
        Write Jupyter Articles  :                     after MER_ART, 3d
        Deep Dive into Polyglot :crit,                2023-04-05, 2w
        Write Polyglot Articles :                     2023-04-12, 10d
    section Delivery
        Final Notebook          :crit, NOTEBOOK,      2023-04-19, 7d
        Rehearsal               :crit,                after NOTEBOOK, 2023-05-04
        Stir Trek 2023          :milestone, crit,     2023-05-05, 1d
```
