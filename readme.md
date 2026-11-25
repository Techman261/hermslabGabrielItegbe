# Hermes Lab – Automate It With Your Own Agent

*INF601 – Advanced Programming in Python* · Gabriel Itegbe

## Overview

This project is about using an AI *agent harness* to do real work, and then comparing it with the one I used
earlier in the course. In the agentic unit I used *Claude Code*, where a model proposes tool calls, the harness
runs them, and the results come back. For this lab I got my own persistent agent, *Hermes*, running in a
container on the course server. I talk to it in two ways:

- *Telegram:* day-to-day chat from my phone or laptop, through a bot I created myself.
- *SSH:* a terminal inside the agent's container, for configuration and files.

Everything the agent does is saved in its container, so it remembers across sessions. The lab has four tasks:

| Task | What | Status |
|------|------|--------|
| 1 | Bring the agent online over my own Telegram bot | Done (Week 14) |
| 2 | Automate three real multi-step tasks | Week 15 |
| 3 | Teach the agent one reusable skill and use it | Done (Week 14) |
| 4 | Write-up: how I directed the agent, Hermes vs. Claude Code, link to my final project | Draft below, finished in Week 15 |

## Repository contents


hermeslabGabrielItegbe/
├── runs/                  # Transcripts, logs, and output files from my tasks
│   ├── task1_online.txt   # Task 1: first exchange with my bot
│   ├── skill_use.txt      # Task 3: creating, using, and refining the skill
│   └── *.md               # Notes the skill produced
├── skill/
│   └── study-note-writer/ # The skill, copied from /opt/data/skills on the server
│       ├── SKILL.md
│       └── templates/
│           └── study-note-writer-template.md
└── README.md


The runs/ and skill/ contents should match what my agent actually did on the server, since the instructor
checks both the repo and the agent's live state.

## Task 1 – Bringing the agent online

1. I connected to my container over SSH from PowerShell, using the login details the instructor gave me.
2. In Telegram, I created my own bot with *@BotFather* and got my numeric user ID from *@userinfobot*.
3. In the container, I edited /opt/data/.env and filled in two values: TELEGRAM_BOT_TOKEN and
   TELEGRAM_ALLOWED_USERS. The second one restricts the bot to my own account.
4. The gateway started on its own within about 15 seconds. I messaged the bot and it answered.

Evidence: runs/task1_online.txt. The bot token is never stored in this repo.

## Task 2 – Three automation tasks

To be completed in Week 15.

| # | Task | Tools the agent used | Evidence |
|---|------|----------------------|----------|
| 1 | TBD | TBD | runs/ |
| 2 | TBD | TBD | runs/ |
| 3 | TBD | TBD | runs/ |

## Task 3 – The reusable skill: study-note-writer

*What it does:* given a topic and raw notes, the skill organizes the notes into 3–5 themed sections, writes a
short Markdown summary with a "Key Takeaways" list, saves it to /opt/data/notes/<topic>.md, and returns the file
path.

*How I built it:* I asked Hermes in Telegram to create the skill. My first request produced no skill. The
agent saved a note instead, so I checked the server (ls /opt/data/skills) and asked again for a real skill folder
with a SKILL.md. Hermes then created /opt/data/skills/study-note-writer/, which I copied into skill/ with
scp.

*Checking the first version:* the generated SKILL.md had the steps I asked for, but it did not use the
front-matter header format of Hermes's built-in skills, and its example usage was a Python call that looked
invented. I treated that as a problem to correct and not as finished work.

*Iteration:* I asked Hermes to update the skill (match the built-in format, remove the invented example, end
every note with a one-line TL;DR, use lowercase-hyphenated filenames), ran it again on a new topic, and checked the
result. (Update this paragraph with what actually changed in your run.)

*Using it:* I ran the skill on the topic "MCP servers" with my own notes. The output note is in runs/, and the
conversation is in runs/skill_use.txt.

## Task 4 – Write-up

### How I directed the agent

My approach was a loop of goal → tool calls → review → correction:

1. Give the agent a clear goal.
2. Read what it did and what it produced.
3. Check the real result on the server, since the agent's description of what it did isn't proof.
4. Correct it and run it again.

Example from Task 3: Hermes reported the skill as done, but my check on the server showed no skill folder, only a
saved note. Asking for the exact path and confirming it with find fixed that. _(Add two or three more examples
from your Week 15 tasks.)_

### Hermes vs. Claude Code

Both are *harnesses* around a model: the model proposes actions, and the harness decides what runs.

| | Hermes | Claude Code |
|---|--------|-------------|
| Where it runs | A container on the course server that I don't control | My own Windows machine |
| How I talk to it | Telegram and SSH | Terminal, and Python through the Agent SDK |
| Memory | Persistent across sessions in its container | Per session, plus project files and skills |
| Skills | Folder under /opt/data/skills/, which the agent can write itself | Folder under .claude/skills/ that I write |
| Permissions | TBD – note what Hermes allowed or asked about | Rules in settings.json; unlisted tools are blocked in headless runs |
| Access | Limited to my Telegram user ID | Local files and any MCP servers I configure |

*What felt similar:* the agent loop, the use of tools, and the idea of a skill as a packaged, repeatable
workflow.

*What differed:* Hermes remembers on its own and is reachable from my phone, but it runs on a server I can't
control, so I can only check what it did through its files and logs. In Claude Code I saw the harness refuse a tool
call until I added it to the allow list (Mini Project 7), which showed me that permissions belong to the harness
and not the model. In Hermes I had to confirm the agent's results myself instead.

(Finish the permissions row and add observations from your Week 15 automation tasks.)

### Connection to my final project

To be completed in Week 15. In my Week 13 outline I proposed an agentic requirement (an MCP server or an Agent
Skill). Here I will say whether this lab confirms or changes that choice, and why. Points to use: persistent
memory, which tools the agent had, how permissions worked, and who controls the environment.

## AI Usage

This whole assignment is AI use, because the point is to direct an AI agent. Here is what each AI did, and what I
did myself.

### Hermes (the agent I was directing)

- *What it provided:* it carried out the tasks I gave it through Telegram. For Task 3 it wrote the
  study-note-writer skill (SKILL.md and a template) and produced the summary notes when I ran the skill.
  (Add what it did in your three Week 15 automation tasks.)
- *What I did:* I wrote the prompts, decided which skill to build, checked its output on the server, noticed that
  the first request saved a note and no skill, pointed out the problems in the first SKILL.md, asked for
  changes, and re-ran it. I also copied the skill and outputs into this repo.

### Claude (Anthropic) in a chat conversation

- *What it provided:* step-by-step guidance and commands for connecting over SSH, creating the Telegram bot,
  editing .env, finding and copying the skill with scp, checking for leaked tokens, and setting up and pushing
  the GitHub repo. It also helped me troubleshoot when the skill wasn't where I expected, and drafted this README.
- *What I did:* I ran every command myself, chose and created my own bot and credentials, kept my token and
  password out of the repo, read the server output to find out what had really happened, and decided what to
  write. I will be able to explain my work and the agent's behavior in the Week 16 review.

### README

AI was also used to help optimize and format this README.md. I reviewed it and edited it so that it matches what
I actually did.