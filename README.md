# agentic-engineering

- Shift tab shift tab to enter plan mode in Claude
- Iterate with claude to have an output document saved to your local repo
- Get your git worktrees setup.
	- Create a command in your .claude/commands/directory
		- e.g. `simple-init-parallel`

Example:
```

```

Invoke the command inside of claude:
`simple-init-parallel support-tagging`

(Add screenshot)

- Create another command to run tasks in parallel:
	- e.g. `exe-parallel`

Example:
```

```

Invoke the command inside of claude:
`exe-parallel my_spec_file.md 3`

(Add screenshot)

## Quality of Life Enhancements
If you do not have a settings.json file, then you will need to grant permissions for tool invocations.

If you want to avoid this, then in your .claude directory, create a settings file. 

For example:


### Minor Hacks
- Make edits to .git/info/exclude in your working repository to ignore files or directories without making changes to your .gitignore

