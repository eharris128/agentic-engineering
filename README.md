# Agentic Engineering

If you think I am talking about vibe coding... I am not.

Don't take it from me though - [Anthropic's Cookbook for Research Lead Engineer](https://github.com/anthropics/anthropic-cookbook/blob/46f21f95981e3633d7b1eac235351de4842cf9f0/patterns/agents/prompts/research_lead_agent.md?plain=1#L135-L137)

## Context Engineering
- [Effective Context Engineering - by Anthropic](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Context Editing](https://docs.claude.com/en/docs/build-with-claude/context-editing)

## Industry Teachers
- [Steve Yegge](https://www.linkedin.com/in/steveyegge)
   	- https://sourcegraph.com/blog/revenge-of-the-junior-developer
	- https://sourcegraph.com/blog/the-brute-squad
- [IndyDevDan](https://www.youtube.com/@indydevdan/videos)

## Helpful Resources
- More Anthropic Cookbook Resources
	- [Subagent OODA](https://github.com/anthropics/anthropic-cookbook/blob/46f21f95981e3633d7b1eac235351de4842cf9f0/patterns/agents/prompts/research_subagent.md?plain=1#L10)
 - Inspiration
 	- [Claude's Research Mode](https://www.anthropic.com/engineering/built-multi-agent-research-system)
  	- [The Creator of Flask's Current Agentic Dev Loop](https://lucumr.pocoo.org/2025/6/12/agentic-coding/)
     		- [Watch him do it live](https://www.youtube.com/watch?v=sQYXZCUvpIc&ab_channel=ArminRonacher) or [here](https://www.youtube.com/watch?v=nfOVgz_omlU&ab_channel=ArminRonacher)
	- [Initial git worktree inspiration](https://www.pulsemcp.com/posts/how-to-use-claude-code-to-wield-coding-agent-clusters)
- MCP Servers
	- [Collaboration among LLMs from Zen MCP Server](https://github.com/BeehiveInnovations/zen-mcp-server)
 	- [Official Figma MCP Guide](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-the-Dev-Mode-MCP-Server)
  	- [Easy screenshot capabilities from Snap Happy](https://github.com/badlogic/lemmy/tree/main/apps/snap-happy)

## Using Claude Code
- Run `/init` within Claude Code to get it familiar with your repository
- Shift tab shift tab to enter plan mode in Claude
	- Use plan mode for any initial inqiury into a bug or a feature

## Git Worktree Based Agentic Dev Loop
- After you work with Claude's plan mode to come up with a spec save it to a specs directory within your repo (e.g. `./specs/new-login-flow.md`)...
- Get your git worktrees setup.
	- Create a command in your `./.claude/commands` directory, still within your repo
		- e.g. `simple-init-parallel.md`
  		- You can whitelist or deny list commands with a `settings.json` file, also within your `.claude` directory. I provide an example below the `simple-init-parallel` example. 


Example:
```
# Simple Init Parallel

# Execute these tasks

Initialize N parallel git worktree directories for concurrent development with full setup including npm dependencies and Claude settings.

## Variables

Parse $ARGUMENTS as "FEATURE_NAME COUNT" where:
- FEATURE_NAME: Name of the feature (required)
- COUNT: Number of worktrees to create (optional, defaults to 3)

Example: "my-feature 5" creates 5 worktrees named my-feature-1 through my-feature-5

## Execute these tasks

CREATE new directory `trees/` if it doesn't exist

> Execute these steps in parallel for concurrency
> 
> Use absolute paths for all commands

For each worktree (1 through COUNT), perform these steps:

CREATE worktree N:
- RUN `git worktree add ./trees/FEATURE_NAME-N` (uses current branch)
- COPY Claude settings:
  - CREATE directory `./trees/FEATURE_NAME-N/.CLAUDE`
  - COPY `.CLAUDE/settings.json` to `./trees/FEATURE_NAME-N/.CLAUDE/settings.json`
  - COPY `.CLAUDE/COMMANDS` directory to `./trees/FEATURE_NAME-N/.CLAUDE/COMMANDS` if it exists
  - COPY `CLAUDE.md` to `./trees/FEATURE_NAME-N/CLAUDE.md`
- INSTALL npm dependencies:
  - RUN `cd ./trees/FEATURE_NAME-N/ui && npm install`
- COPY environmental variables:
  - COPY `ui/.env` to `./trees/FEATURE_NAME-N/ui/.env`
- SET PORT=300N for React app (use `PORT=300N npm start` when running)
- VALIDATE setup:
  - CHECK that `./trees/FEATURE_NAME-N/ui/node_modules` exists
  - CHECK that `./trees/FEATURE_NAME-N/.CLAUDE/settings.json` exists
  - CHECK that `./trees/FEATURE_NAME-N/ui/.env` exists

VERIFY all worktrees by running `git worktree list`

REPORT summary of created worktrees with validation status

## Running the worktrees

For each worktree, start frontend with:
- Frontend: `cd trees/FEATURE_NAME-N/ui && PORT=300N npm start`

Where N is the worktree number (1 through COUNT)

Note: Port numbers are calculated as 3000 + N (e.g., worktree 1 uses port 3001, worktree 5 uses port 3005)

## Notes

- npm install runs automatically during setup
- Claude settings are copied to ensure agents have proper permissions
- Each worktree uses the current branch (not creating new branches)
- If COUNT is not specified, defaults to 3 worktrees
- Maximum recommended worktrees: 10 (to avoid port conflicts and resource constraints)

## Auto-Approved Commands

The `settings.json` file copied to each worktree configures which commands agents can execute without prompting for approval. This includes:

### NPM Commands
- `npm install`
- `npm run lint`
- `npm run lint:fix`
- `npm test`
- `npm run test`
- `npm run build`

### File Operations
- RESULTS.md creation and editing
- Reading any file for context
- Editing files within assigned worktree
- Creating new files within assigned worktree
- Modifying source code files (.js, .jsx, .ts, .tsx, .css, .scss)
- Updating configuration files within worktree

### Git Commands
- `git status`
- `git diff`
- `git log`

### Bash/File System Commands
- `true` (no-op command)
- `ls` (list directory contents)
- `pwd` (print working directory)
- `cd` (change directory within worktree)
- `find` (search for files within worktree)
- `cat` (view file contents)
- `head` (view beginning of files)
- `tail` (view end of files)
- `echo` (print text)
- `tree` (show directory structure)
- `which` (locate commands)
- `dirname` (extract directory from path)
- `basename` (extract filename from path)
- `realpath` (resolve absolute path)

All file system operations are constrained to the assigned worktree directory to maintain isolation between concurrent development sessions.
```

Invoke the command inside of claude:
`simple-init-parallel new-login-flow 3`

- Your Claude Code should now be creating a `./trees` directory & copying over the relevant files into each of the `./trees/new-login-button-1` subdirectories.

- Once this step is complete, you should have:
	-  Three `new-login-button-N` directories with all relevant dependencies inside of your trees directory.

## Implementing the spec
- You have your spec. You have your repository copies. Now you just need Claude to iterate on them.

- Create another command to have Claude iterate on each worktree in parallel:
	- e.g. `exe-parallel add-new-button 3`

Example:
```
# Parallel Task Version Execution

Execute a specification from the ./specs/ directory across multiple parallel worktrees.

## Prerequisites
Worktrees must be created first using: `/simple-init-parallel SPEC_NAME COUNT`

## Usage Examples
- `/exe-parallel grid-v2` (uses 3 worktrees by default)
- `/exe-parallel grid-v2 3` (explicitly uses 3 worktrees)
- `/exe-parallel design-tidy 5` (uses 5 worktrees)

## Variables
SPEC_NAME: $ARGUMENTS[0] (name of the spec file to execute)
NUMBER_OF_PARALLEL_WORKTREES: $ARGUMENTS[1] (number of parallel worktrees to use, defaults to 3 if not provided)

## Spec Loading
NAVIGATE to the root of the project
CHECK if `./specs/{SPEC_NAME}.md` exists
  - IF exists: READ the contents of `./specs/{SPEC_NAME}.md` into PLAN_TO_EXECUTE
  - IF NOT exists: 
    - ERROR: "Spec file '{SPEC_NAME}.md' not found in ./specs/ directory"
    - LIST all available specs: RUN `ls ./specs/`
    - EXIT with error

SET NUMBER_OF_PARALLEL_WORKTREES to 3 if not provided in $ARGUMENTS[1]

## Worktree Validation
If worktrees don't exist for the given SPEC_NAME:
- INFORM user to run `/simple-init-parallel {SPEC_NAME} {NUMBER_OF_PARALLEL_WORKTREES}` first
- EXIT with helpful error message

## Pre-execution validation

RUN `ls -la trees/`

For each worktree (1 to NUMBER_OF_PARALLEL_WORKTREES):
- VERIFY worktree exists at `./trees/{SPEC_NAME}-N`
- CHECK that `./trees/{SPEC_NAME}-N/ui/node_modules` exists
  - IF missing, RUN `cd ./trees/{SPEC_NAME}-N/ui && npm install`
- CHECK that `./trees/{SPEC_NAME}-N/.CLAUDE/settings.json` exists
  - IF missing, COPY from `.CLAUDE/settings.json`
- CHECK that `./trees/{SPEC_NAME}-N/CLAUDE.md` exists
  - IF missing, COPY from `./CLAUDE.md`

READ: PLAN_TO_EXECUTE

## Instructions

We're going to create NUMBER_OF_PARALLEL_WORKTREES new subagents that use the Task tool to create N versions of the same feature in parallel.

This enables use to concurrently build the same feature in parallel so we can test and validate each subagent's changes in isolation then pick the best changes.

The first agent will run in trees/{SPEC_NAME}-1/
The second agent will run in trees/{SPEC_NAME}-2/
...
The last agent will run in trees/{SPEC_NAME}-NUMBER_OF_PARALLEL_WORKTREES}/

The code in trees/{SPEC_NAME}-<i>/ will be identical to the code in the current branch. It will be setup and ready for you to build the feature end to end.

Each agent will independently implement the engineering plan detailed in PLAN_TO_EXECUTE

**IMPORTANT PERMISSION CONSTRAINTS FOR EACH AGENT:**
- Agent 1: ONLY edit files within trees/{SPEC_NAME}-1/
- Agent 2: ONLY edit files within trees/{SPEC_NAME}-2/
- Agent N: ONLY edit files within trees/{SPEC_NAME}-N/
- DO NOT edit any files outside your assigned worktree directory
- DO NOT modify any files in the parent directories or other worktrees

**AUTO-APPROVAL FOR COMMON OPERATIONS:**
Each agent should follow the auto-approval rules defined in `.CLAUDE/settings.json` which covers npm commands, file operations, and git operations without requiring user prompts.

When the subagent completes it's work:

1. **DEPENDENCY CHECK**: Ensure node_modules exists, run `npm install` if needed
2. **LINTING REQUIREMENT**: The subagent MUST run `npm run lint` in the `ui/` directory of their worktree
3. If linting errors are found, the subagent MUST fix all breaking issues before proceeding
4. The subagent should run `npm run lint:fix` to automatically fix what can be fixed
5. Any remaining lint errors must be manually resolved
6. Only after all lint errors are resolved should the subagent create the `RESULTS.md` file

The subagent should report their final changes made in a comprehensive `RESULTS.md` file at the root of their respective workspace. The RESULTS.md should include:
- Summary of all changes made
- Confirmation that linting passed successfully
- Any lint issues that were fixed
- Files modified
- Any setup issues encountered and resolved

Make sure agents don't run start.sh or any other script that would start the server or client - focus on the code changes only.

**PORT ASSIGNMENTS:**
If agents need to test their changes by running the development server:
- Agent 1: Use `PORT=3001 npm start` in trees/{SPEC_NAME}-1/ui/
- Agent 2: Use `PORT=3002 npm start` in trees/{SPEC_NAME}-2/ui/
- Agent N: Use `PORT=300N npm start` in trees/{SPEC_NAME}-N/ui/

This ensures each worktree runs on a different port to avoid conflicts.

## Post-execution summary

After all agents complete:
- COLLECT all RESULTS.md files
- SUMMARIZE the different approaches taken
- IDENTIFY the most successful implementation
- REPORT any common issues or patterns observed
```

- Invoke the command inside of claude:
`exe-parallel my_spec_file.md 3`

## Quality of Life Enhancements
If you do not have a settings.json file, then you will need to grant permissions for tool invocations.

If you want to avoid this, then in your .claude directory, create a settings file. 

For example:
```
{
  "permissions": {
    "allow": [
      "Bash(grep:*)",
      "Bash(rg:*)",
      "Bash(git worktree:*)",
      "Bash(cp:*)",
      "Bash(npm test:*)",
      "Bash(ls:*)",
      "Bash(npx react-scripts test:*)",
      "Bash(node:*)",
      "Bash(npm run lint:*)",
      "Bash(chmod:*)",
      "Bash(./start_single_tree.sh:*)",
      "Bash(./install_worktree_deps.sh:*)",
      "Bash(./check_all_lint.sh)",
      "Bash(kill:*)",
      "Bash(find:*)",
      "Bash(mkdir:*)",
      "Bash(npm run lint:fix:*)",
      "Bash(npm run build:*)",
      "Bash(npm run typecheck:*)",
      "Bash(python manage.py test:*)",
      "Bash(python manage.py check:*)",
      "Bash(cd:*)",
      "Bash(pwd:*)",
      "Bash(cat:*)",
      "Bash(head:*)",
      "Bash(tail:*)",
      "Bash(wc:*)",
      "Bash(diff:*)",
      "Bash(git diff:*)",
      "Bash(git status:*)",
      "Bash(git log:*)",
      "Bash(npm install:*)",
      "Bash(pip install:*)",
      "Bash(echo:*)",
      "Bash(rm:*)",
      "Bash(mv:*)",
      "Bash(touch:*)",
      "Bash(sed:*)",
      "Bash(awk:*)",
      "Bash(jq:*)",
      "Bash(curl:*)",
      "Bash(which:*)",
      "Bash(npm run test -- --coverage:*)",
      "Bash(npm audit:*)",
      "Bash(npm outdated:*)",
      "Bash(ps:*)",
      "Bash(lsof:*)",
      "Bash(netstat:*)",
      "Bash(env:*)",
      "Bash(export:*)",
      "Bash(source:*)",
      "Bash(bash:*)",
      "Bash(sh:*)",
      "Bash(make:*)",
      "Bash(docker:*)",
      "Bash(tree:*)",
      "Bash(du:*)",
      "Bash(df:*)",
      "Bash(npm run lint -- --fix:*)",
      "Bash(eslint:*)",
      "Bash(prettier:*)",
      "Bash(jest:*)",
      "Bash(tsc:*)",
      "Bash(python -m:*)",
      "Bash(pip freeze:*)",
      "Bash(git add:*)",
      "Bash(git commit:*)",
      "Bash(git push:*)",
      "Bash(git pull:*)",
      "Bash(git checkout:*)",
      "Bash(git branch:*)",
      "Bash(npm run:*)",
      "Bash(yarn:*)",
      "Bash(pnpm:*)",
      "Bash(npx:*)",
      "Bash(./scripts/*:*)",
      "Bash(python manage.py:*)",
      "Bash(coverage:*)",
      "Bash(pytest:*)",
      "Bash(flake8:*)",
      "Bash(black:*)",
      "Bash(isort:*)",
      "Bash(mypy:*)",
      "Bash(ruff:*)"
    ],
    "deny": [
      "Bash(rm -rf /)",
      "Bash(sudo:*)",
      "Bash(su:*)",
      "Bash(chmod 777:*)",
      "Bash(npm publish:*)",
      "Bash(pip upload:*)",
      "Bash(git push --force:*)",
      "Bash(git reset --hard:*)",
      "Bash(> /dev/null 2>&1:*)",
      "Bash(nohup:*)",
      "Bash(screen:*)",
      "Bash(tmux:*)",
      "Bash(&:*)",
      "Bash(cron:*)",
      "Bash(at:*)",
      "Bash(systemctl:*)",
      "Bash(service:*)",
      "Bash(reboot:*)",
      "Bash(shutdown:*)",
      "Bash(passwd:*)",
      "Bash(useradd:*)",
      "Bash(userdel:*)",
      "Bash(groupadd:*)",
      "Bash(chown:*)",
      "Bash(mount:*)",
      "Bash(umount:*)",
      "Bash(fdisk:*)",
      "Bash(dd:*)",
      "Bash(nc:*)",
      "Bash(telnet:*)",
      "Bash(ssh:*)",
      "Bash(scp:*)",
      "Bash(rsync:*)",
      "Bash(wget:*)",
      "Bash(apt:*)",
      "Bash(yum:*)",
      "Bash(brew:*)",
      "Bash(npm config:*)",
      "Bash(git config --global:*)",
      "Bash(pip config:*)",
      "Bash(rm -rf trees/)",
      "Bash(rm -rf .git/)",
      "Bash(rm -rf node_modules/)",
      "Bash(killall:*)",
      "Bash(pkill:*)",
      "Bash(:(){ :|:& };:)",
      "Bash(eval:*)",
      "Bash(exec:*)"
    ]
  }
}
```


### Minor Hacks
- Make edits to .git/info/exclude in your working repository to ignore files or directories without making changes to your .gitignore

## Additional Resoruces
- [Google approach to secure AI Agents](https://research.google/pubs/an-introduction-to-googles-approach-for-secure-ai-agents/)
- [Cursor team take on building on top of LLM's YT](https://x.com/alexalbert__/status/1932457221507695041)
- [React hook](https://github.com/modelcontextprotocol/use-mcp)
- [Adding task management ontop of Claude Code from Within Cursor](https://github.com/eyaltoledano/claude-task-master)
- [AI Agent Infra - Gitpod](https://www.gitpod.io/blog/ai-agents-need-infrastructure)
- 

### Mastering Claude Code
- [Rolling through tech debt with ease](https://blog.puzzmo.com/posts/2025/07/30/six-weeks-of-claude-code/)

### Building MCP Servers
- [MCP Server Design Best Practices](https://engineering.block.xyz/blog/blocks-playbook-for-designing-mcp-servers)
- [OAuth Advice for MCP Servers (TLDR - do not roll your own)](https://den.dev/blog/mcp-prm-auth/)

#### MCP Secutity

- [Security Best Practices](https://modelcontextprotocol.io/specification/2025-06-18/basic/security_best_practices)
- [Auth on MCP](https://aws.amazon.com/blogs/opensource/open-protocols-for-agent-interoperability-part-2-authentication-on-mcp/)

## Closing Comments
- In my naive claude commands, you can see a spread between these initial ones and how Anthropic structures their prompts.
- I run `npm install` fresh for each worktree due to issues to with symlinking. I aspire to use [Container Use](https://github.com/dagger/container-use) to remediate this issue. Start simple though.
- The `settings.json` needs refinement and may or may not work as intended. People have different opinions on `allowing all` or not. I actively hack MCP Servers (most are compromisable) so I default to not allowing all - YMMV.
