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



---



# Recipes
```bash
# summarize folders
cat devlog/* | ./chat --prompt 'summarize this folder'

# 

# summarize git changes, copy output into clipboard
git diff | ./chat --prompt 'summzarise this diff as a single line git commit message. Wrap in quotes, start with YYMMDD' | tee /dev/tty | tr -d '\n' | xsel -ib
```
