---
name: study-note-writer
description: "Organizes raw notes into themed sections and generates a summary with key takeaways for a given topic."
license: Proprietary. LICENSE.txt has complete terms
platforms: [linux]
---

# Study Note Writer Skill

## Description
This skill helps in organizing raw notes into themed sections and generating a summary with key takeaways for a given topic. It saves the summary to a Markdown file and provides the file path.

## Steps
1. Receive the topic and raw notes.
2. Organize the notes into 3-5 themed sections.
3. Write a short Markdown summary including a "Key Takeaways" list.
4. Add a one-line TL;DR at the end of each section.
5. Save the summary to `/opt/data/notes/<topic>.md` where `<topic>` is converted to lowercase and hyphenated.
6. Provide the file path to the user.

## Example Usage
- Topic: Any given topic
- Raw Notes: Raw notes provided by the user