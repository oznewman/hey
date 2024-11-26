# ./hey/chat.sh
## A portable swarm agent framework written in Bash

This repo has everything you need to design your own agents, swarms, and mixture of experts with minimal code. You can chat with it, pipe in content from other scripts, and redirect the output to the terminal (default) or anywhere else (including itself)

I made this because neither Ollama nor Llama.cpp allow for easily piping in files. They also don't allow for easy model swapping mid-conversation, or for chaining prompts together...this script allows for all that and more!


---



## Flags
```bash
## Flags
--hostname  # [openrouter|groq]
--more      # pipe into more command
--model     # model ID to use (depends on hostname)
--prompt    # the prompt to use
--system    # the system prompt to use
```

### Notes
- If the first argument is a string, it is assumed that that is a prompt
- If no model flag is passed, it is assumed to try the state of the art generalist model on openrouter

```bash
# These are all similar:
./hey --prompt 'hello how are you' --model 'sota' --hostname 'openrouter'
./hey --prompt 'hello how are you' --model 'sota'
./hey 'hello how are you' --sota
./hey 'hello how are you'
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
