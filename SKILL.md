---
name: "dynamic-conversation-recap"
description: "Automatically detects lost conversational context and provides an immediate summary."
---

# Skill: Dynamic Conversation Recap

## Description
This skill enables an OpenClaw agent to automatically detect when a conversational context has been lost (e.g., due to client-side resets, long periods of inactivity, or new session starts) and proactively provides a concise summary of recent interactions to the user. This ensures that both the user and the agent immediately regain shared understanding, eliminating the need for manual prompts to "catch up."

## Implementation Logic

The skill operates by integrating a context-detection mechanism into the agent's message processing flow. For every new inbound message, the agent performs the following steps:

1.  **Context Gap Detection:**
    *   Examine the immediate session history available in the current turn.
    *   Evaluate if the history is substantially short (e.g., fewer than a configurable threshold of messages) *and* if there has been a significant time elapsed since the last interaction (e.g., several hours, indicating a potential session reset or long break).
    *   These thresholds for "short history" and "significant time elapsed" are internal parameters the agent can adjust.

2.  **Proactive Context Retrieval:**
    *   If a context gap is detected, the agent uses the `sessions_history` tool to fetch a configurable number of the most recent messages (e.g., the last 50-100 messages) from the relevant session. This ensures a broad enough context for summarization.

3.  **Summary Generation:**
    *   The retrieved messages are then fed into the agent's language model to synthesize a concise and relevant summary. This summary focuses on ongoing tasks, key decisions, and the last known state of active discussions.

4.  **Delivery to User:**
    *   The generated summary is then delivered to the user in the current chat channel using the `message` tool. This acts as an immediate "briefing" to restore continuity for both the user and the agent.

## Benefits
*   **Enhanced User Experience:** Eliminates the frustration of repeating context or feeling like the agent has "forgotten" previous discussions.
*   **Increased Efficiency:** Reduces the need for manual context-setting at the start of new interactions.
*   **Seamless Continuity:** Ensures a smoother and more natural conversational flow across extended periods or session interruptions.

## Usage Considerations
*   The effectiveness relies on appropriate internal thresholds for context gap detection.
*   Token usage for summarization should be monitored, especially if very long histories are retrieved.
*   The skill can be adapted to various chat platforms where direct cron announcements might not be feasible, leveraging the agent's ability to process internal events and use the `message` tool for delivery.
