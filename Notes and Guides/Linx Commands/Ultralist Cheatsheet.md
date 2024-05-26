Certainly, Netrunner! Below is a detailed cheat sheet for Ultralist formatted in Markdown with Font Awesome icons and callouts, suitable for Obsidian. 


# Ultralist CLI Cheat Sheet


### Adding a Task ➕
```sh
ultralist add some important task for the +project due tom
```
Example:
```sh
ultralist add chat with @bob about +specialProject due:tom
```

### Listing Tasks 📄
```sh
ultralist list
```
Example output:
```
all
1  [ ]  tomorrow    some important task
```

### Marking a Task Completed ✅
```sh
ultralist c 1
```
Example output:
```
todo completed.
```

### Archiving a Task 🗄️
```sh
ultralist ar 1
```
Example output:
```
todo archived.
```

## Managing Todos

### Listing, Filtering, and Grouping Todos

- **List all tasks**: `ultralist list` or `ultralist l`
- **Filter by due date**: `ultralist l due:tod` (today), `ultralist l due:tom` (tomorrow)
- **Filter by completion**: `ultralist l completed:true` (completed tasks)
- **Filter by project**: `ultralist l project:mobile`

### Task Due Date Format

- **Today/Tomorrow**: `due:today`, `due:tom`
- **Specific Date**: `due:may2` or `due:2may`

### Completing/Uncompleting Todos
```sh
ultralist complete [id]  # or ultralist c [id]
ultralist uncomplete [id]  # or ultralist uc [id]
```

### Archiving/Unarchiving Todos
```sh
ultralist archive [id]  # or ultralist ar [id]
ultralist unarchive [id]  # or ultralist uar [id]
ultralist ar c  # archive all completed todos
ultralist ar gc  # garbage collect
```

### Prioritizing/Unprioritizing Todos
```sh
ultralist prioritize [id]  # or ultralist p [id]
ultralist unprioritize [id]  # or ultralist up [id]
```

### Deleting Todos
```sh
ultralist delete [id]  # or ultralist d [id]
```

### Editing Todos
```sh
ultralist e [id] <subject> <due:[due]> <status:[status]> <completed:[true|false]> <priority:[true|false]> <archived:[true|false]>
```

### Adding Notes to Todos
```sh
ultralist addnote <todoId> <content>  # or ultralist an <todoId> <content>
```

### Editing Notes
```sh
ultralist editnote <todoId> <noteId> <content>  # or ultralist en <todoId> <noteId> <content>
```

### Deleting Notes
```sh
ultralist deletenote <todoId> <noteId>  # or ultralist dn <todoId> <noteId>
```

## Task Recurrence 🔄
```sh
ultralist add Daily standup recur:weekdays
ultralist edit 3 recur:monthly
ultralist e 23 recur:none  # cancel recurrence
ultralist add Work on +budget planning recur:weekly until:dec5
```

## Showing Tasks

### Filters
- **Due**: `due:(tod|today|tom|tomorrow|thisweek|nextweek|lastweek|mon|tue|wed|thu|fri|sat|sun|none|<specific date>)`
- **Completion**: `completed:true` or `completed:false`
- **Priority**: `priority:true` or `priority:false`
- **Project**: `project:mobile`
- **Status**: `status:now`

### Examples
```sh
ultralist l due:tod  # show tasks due today
ultralist l duebefore:tom  # show tasks due before tomorrow
ultralist l completed:true  # show only completed tasks
ultralist l completed:false  # show only incomplete tasks
ultralist l project:mobile  # show tasks with a project of mobile
ultralist l group:project  # list all tasks, grouped by project
ultralist l group:context  # list all tasks, grouped by context
```

## Best Practices 💡

- **Avoid Overdue Tasks**: Complete, reschedule, or delete overdue tasks.
- **Use the Agenda View**: `ultralist l due:agenda`
- **Create Shell Aliases**:
  ```sh
  alias u="ultralist"
  alias uc="ultralist l due:agenda group:context"
  alias up="ultralist l due:agenda group:project"
  alias tod="ultralist l group:project due:tod"
  alias tom="ultralist l group:project due:tom"
  alias mon="ultralist l group:project due:mon"
  alias tue="ultralist l group:project due:tue"
  alias wed="ultralist l group:project due:wed"
  alias thu="ultralist l group:project due:thu"
  alias fri="ultralist l group:project due:fri"
  alias c="ultralist l completed:tod"
  alias uf="script -c \"ultralist l\" < /dev/null | fzf --ansi"
  ```
- **Show Ultralist at Shell Startup**: Add `ultralist list` to the end of your `.zshrc` or `.bashrc`.
- **Sync with Cron**:
  ```sh
  */15 8-17 * * 1-5 cd ~/work && ultralist sync
  ```

## File Format 📂

Ultralist stores tasks in a `.todos.json` file in the directory where `ultralist init` was run. If `.todos.json` is not found in the current directory, it will read from the home directory.

### Example `.todos.json`
```json
[
  {
    "id": 1,
    "uuid": "55dd9b2d-5bdf-4d4c-b9b7-73e83d94ddb2",
    "subject": "support for priorities +feature",
    "projects": ["feature"],
    "contexts": [],
    "due": "2016-07-08",
    "completed": false,
    "completedDate": "",
    "archived": false,
    "isPriority": false,
    "notes": ["this is a note"]
  },
  {
    "id": 2,
    "uuid": "8cf1083e-a230-454f-82f3-49ebee2fc40d",
    "subject": "fix bug with 'tod' not being recognized +bug",
    "projects": ["bug"],
    "contexts": [],
    "due": "2016-07-08",
    "completed": false,
    "completedDate": "",
    "archived": false,
    "isPriority": false,
    "notes": null
  }
]
```
