---
title: "This Post was created entirely by AI"
date: 2025-07-25
draft: false
featuredImage: feature3.webp
---
![AIBLOGGING](feature3.webp)

## Unleash Local AI: Running Gemma with Memory on Your Linux Terminal 🧠

Have you ever dreamed of having a personal AI assistant, accessible directly from your terminal, without relying on cloud services or complex chat interfaces? I did, and I turned that dream into reality by running the Gemma 3B language model locally, complete with a basic memory system. This post details my journey, offering a practical guide and a starting point for your own local AI experiments.

---

## 🚀 What I Set Out to Do

I wanted to create a truly local AI assistant. The core goals were ambitious:

*   **Offline Operation:**  Run the AI completely offline, removing dependency on internet connectivity.
*   **Memory Retention:**  Simulate memory, allowing the assistant to recall previous conversations and facts.
*   **Contextual Responses:**  Generate responses informed by the conversation history and injected facts.
*   **Terminal-Based Interaction:**  Control the assistant entirely through the command line.

Essentially, I wanted a nimble, private, and customizable AI companion right at my fingertips.

---

## 🛠️ Tools I Used

The success of this project hinged on a few key tools:

*   **[Ollama](https://ollama.com/)** – This incredible tool is the cornerstone. It streamlines the process of downloading, running, and managing large language models like Gemma locally.  Ollama handles everything from model files to execution, making it dead simple to get started, even for those unfamiliar with the technical details. It's the easiest way to get a model like Gemma up and running quickly.
*   **Gemma 3B (4-bit)** – Google’s Gemma 3B model was selected for its lightweight nature and surprisingly strong performance, especially given its size.  The 4-bit quantization significantly reduces memory requirements, making it feasible to run on a relatively modest Linux machine.
*   **Bash Scripting** –  I leveraged the power of Bash to extend Gemma's functionality, enabling memory management, custom commands, and a more interactive experience.
*   **Aliases** – For quick access and simplified command syntax, I implemented aliases within my shell configuration.

---

## 🔧 Step-by-Step Summary

Let's break down the process into manageable steps:

### 1. **Install Ollama**

First things first, you need Ollama installed. Open your terminal and paste the following command:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

This script automatically downloads and installs Ollama.  After completion, you’ll have a powerful tool ready to deploy your local AI assistant.

> ✅ Ollama manages local model files, handles execution, and makes it dead simple to run AI models offline.

---

### 2. **Download the Gemma Model**

Now, let's get Gemma running.  Execute the following command within your terminal:

```bash
ollama run gemma3:4b
```

This command downloads the Gemma 3B 4-bit model. The first run will pull around 2GB of manifest and model data.  Once downloaded, the model is available offline, ready for use.

---

### 3. **Set Up Memory System**

To simulate memory – as models don't inherently retain context between runs – I created a script to manage conversation history and injected facts. This system lets Gemma “remember” our interactions.

**Script (`~/.local/bin/gemma-memory`):**

```bash
#!/bin/bash

FACTS_FILE="$HOME/.gemma_memory/facts.txt"
HISTORY_FILE="$HOME/.gemma_memory/history.log"
MAX_SIZE=104857600  # 100MB max size for history

# Ensure memory directory exists
mkdir -p "$HOME/.gemma_memory"
touch "$FACTS_FILE" "$HISTORY_FILE"

# Read input
PROMPT="$*"

# Build prompt with memory
FULL_PROMPT="Facts:\n$(cat "$FACTS_FILE")\n\nConversation History:\n$(tail -n 50 "$HISTORY_FILE")\n\nYou:\n$PROMPT\nGemma:"

# Run the model
RESPONSE=$(echo -e "$FULL_PROMPT" | ollama run gemma3:4b)

# Output + save
echo "$RESPONSE"
echo -e "You: $PROMPT\nGemma: $RESPONSE\n" >> "$HISTORY_FILE"

# Trim history if oversized
if [ -f "$HISTORY_FILE" ] && [ $(stat -c%s "$HISTORY_FILE") -gt $MAX_SIZE ]; then
    tail -n 500 "$HISTORY_FILE" > "${HISTORY_FILE}.tmp" && mv "${HISTORY_FILE}.tmp" "$HISTORY_FILE"
fi
```

Make the script executable:

```bash
chmod +x ~/.local/bin/gemma-memory
```

This script loads persistent facts, references recent conversations, and feeds them into the model's prompt for contextual responses.

---

### 4. **Create Aliases**

To streamline interactions, I created terminal aliases:

Edit `~/.bashrc` or `~/.zshrc`:

```bash
alias gemma="ollama run gemma3:4b"
alias gemma-memory="~/.local/bin/gemma-memory"
```

Apply changes:

```bash
source ~/.bashrc
```

Now I can interact with Gemma using:

*   `gemma "What is AI?"` – A standard prompt without memory.
*   `gemma-memory "What did I tell you last time?"` –  Utilizes the memory system for context.

---

### 5. **Testing Memory**

Let's test the memory system's persistence:

```bash
gemma-memory "My name is Dio."
# Appends to history.log

gemma-memory "What's my name?"
# Uses memory to reply "Dio"
```

To manually add facts, execute:

```bash
echo "My favorite band is Queen." >> ~/.gemma_memory/facts.txt
```

This demonstrates how you can directly influence the AI's knowledge base.

## 💭 Limitations

It’s crucial to understand the constraints of this setup:

*   **Simulated Memory:**  Gemma itself doesn't "learn" or retain memory in a traditional sense. This system merely provides a mechanism for feeding relevant data into each prompt.
*   **Manual Maintenance:** You're responsible for maintaining `facts.txt` and `history.log`.
*   **No True Retention:**  Models hallucinate facts, especially with vague prompts and minimal input.

## 🧪 What Else You Can Do

Now that Gemma is running with memory, here are some ideas:

*   Create a `blog.sh` script to generate markdown from prompts.
*   Extract “important facts” from logs and append to memory automatically.
*   Add `readfile.sh` to summarize or explain local files via `cat file.md | gemma-memory`.

## 📎 Final Thoughts

Running Gemma locally like this transforms your terminal into a lightweight, privacy-respecting AI assistant – totally offline, customizable, and fun to hack on.  The memory system is basic but opens the door to bigger things: local agents, journaling assistants, smart shells, and more.

**Key Benefits:**

*   **Privacy:** Your data stays on your machine.
*   **Offline Access:** No internet connection required.
*   **Customization:** Tailor the experience to your specific needs.
*   **Learning:**  A fantastic project for learning about LLMs and prompt engineering.

Make sure the final word count meets requirements and this document is more than 800 words.


