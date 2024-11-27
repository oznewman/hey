# A portable swarm agent framework written in Bash

This repo has everything you need to design your own agents, swarms, and mixture of experts with minimal code. You can chat with it, pipe in content from other scripts, and redirect the output to the terminal (default) or anywhere else (including itself)

---

# Commands && Flags
## ./hey [prompt] --flags
#### Flags and basic usage

```bash
## Flags
--prompt    # (optional) The prompt to use
--system    # (optional) the system prompt to use
--hostname  # [openrouter|groq]
--model     # model ID to use (depends on hostname)
--loop      # enters an interactive chat
--pager     # use a pager for long responses
```

- If the first argument is a string, it is assumed that argument is the `--prompt`
- If the first and second arguments are strings, then the second one is assumed to be `--system`
- If no `--model` flag is passed, it will try to use the most generally accepted state of the art model available
- If no `--hostname` flag is passed, it is assumed to use [OpenRouter.ai](https://openrouter.ai)
- All defaults are configurable in `./hey/defaults`

```bash
## --model shorthands
--sota     # state of the art available in --hostname
--search   # use a search model, like perplexity
--flash    # use a fast model, like claude haiku
--rolepla  # roleplaying model
--small    # use the smallest available model, like qwen 2.5 250M

# These are all similar:
./hey --prompt 'hello how are you' --model 'anthropic/claude-3.5-sonnet' --hostname 'openrouter'
./hey --sota --prompt 'hello how are you'
./hey 'hello how are you'
```

### Creating chains
#### Piping extra context
```bash
# Quickly summarize things
echo "$VARIABLE"
| chat "what is the value of that variable?"

# Files
cat file.txt
| chat "summarize the file"

# Directories
cat devlog/2411*.md
| chat "extract tasks that I missed into a new list"

# Summarizing chat sessions
cat devlog/*
| ./hey 'start a new life coaching session with summary and question' --loop
| ./hey 'summarize the session'
>> coaching-notes.txt
```



---



# Recipes
```bash
# Summarize october
echo "<aboutme>$(cat oznewman/**/*)</aboutme>
  <devlog>$(cat devlog/2410*.md)</devlog>" |
./hey/chat "summarize these notes" --more

# Prioritize todo's
echo "<aboutme>$(cat oznewman/**/*)</aboutme>
  <devlog>$(cat devlog/**/*)</devlog>" |
./hey/chat "extract and prioritize todos" --more

# Improve source code
echo "<code>$(cat hey/**/*)</code>" |
./hey/chat "That is your source code, how would you improve it?" --more

# Summarize git changes, copy output into clipboard
git diff |
bash $NEWMAN/hey/chat 'summarize git changes as a single line commit message. Wrap in quotes, start with YYMMDD' |
tee /dev/tty |
tr -d '\n' |
xsel -ib
```
