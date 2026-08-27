# Code Humanizer

Claude Code skill that rewrites AI-generated code to look human-written — less over-engineered, easier to read and maintain.

## Origin

Inspired by [Humanizer](https://github.com/blader/humanizer). This started as a Gist when Humanizer had ~1.5k stars; it's since passed 34k and turned into an AI-agent project. This is a separate implementation, scoped to code only.

## Before / After

Before:
```python
def process_user_data(user_data):
    # Check if user data exists before processing
    if user_data is not None:
        # Iterate through each user in the provided data
        for user in user_data:
            # Process the user if they are active
            if user.get("active") == True:
                process_user(user)
```

After:
```python
def process_users(users):
    for user in users or []:
        if user.get("active"):
            process_user(user)
```

Goal: strip ceremony, redundant comments, and over-abstraction — not just shorten line count.

## Install

Copy the `skills/code-humanizer` directory into your Claude Code skills directory:

- **Personal:** `~/.claude/skills/code-humanizer/`
- **Project:** `.claude/skills/code-humanizer/`

Claude Code picks up the skill from there automatically.

## Usage

Claude Code applies this skill when generating, refactoring, or reviewing code where a natural, human-written style is wanted.

## Credit

Inspired by [Humanizer](https://github.com/blader/humanizer) (blader). Independent implementation, code-focused.
