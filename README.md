# task-73-dheepikaag
import os
import json
from groq import Groq

# -----------------------------
# Configuration
# -----------------------------
API_KEY = "YOUR_GROQ_API_KEY"   # Replace with your Groq API key
MODEL = "llama3-8b-8192"
MEMORY_FILE = "conversation.json"

client = Groq(api_key=API_KEY)

# -----------------------------
# Load conversation history
# -----------------------------
def load_memory():
    if os.path.exists(MEMORY_FILE):
        with open(MEMORY_FILE, "r") as f:
            return json.load(f)
    return []

# -----------------------------
# Save conversation history
# -----------------------------
def save_memory(messages):
    with open(MEMORY_FILE, "w") as f:
        json.dump(messages, f, indent=4)

# -----------------------------
# Main Chat Loop
# -----------------------------
messages = load_memory()

print("Chatbot with Memory")
print("Type 'exit' to quit.\n")

while True:
    user_input = input("You: ")

    if user_input.lower() == "exit":
        break

    # Append user message
    messages.append({
        "role": "user",
        "content": user_input
    })

    # Send full history to Groq
    response = client.chat.completions.create(
        model=MODEL,
        messages=messages
    )

    assistant_reply = response.choices[0].message.content

    print("Bot:", assistant_reply)

    # Append assistant message
    messages.append({
        "role": "assistant",
        "content": assistant_reply
    })

    # Save updated conversation
    save_memory(messages)