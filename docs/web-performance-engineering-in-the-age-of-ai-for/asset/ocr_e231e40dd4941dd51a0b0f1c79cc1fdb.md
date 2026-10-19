This is the fraction of time during a stream when no tokens arrive for more than a threshold duration (typically 200 ms or more). Frequent stalls make an AI feel unreliable or struggling, even if total completion time is reasonable. A smooth, consistent flow feels more intelligent and confident.

# Time to last token

Time to last token (TTLT) is the total time from prompt submission to receiving the final token. While less critical than in traditional page loads (since users get progressive value), it still matters for completion and for planning follow-up interactions.

# UI commit latency

This is the time from when a token arrives from the network to when it appears rendered on screen. High UI commit latency means users see stuttering text or delays in updates, degrading the feel of real-time streaming.

# Performance Budgets

Based on research into user perception and real-world deployments, here are starting budgets you can tune for your application:

# First token under 500 ms (warm path)

On subsequent interactions in a session, when connections are already established and the model might have cached context, aim for the first token in under half a second. This maintains the conversational feel.

# First token under 1 second (cold path)

On initial requests when everything is cold—no connection reuse, no cached context—one second is a reasonable target. Users are typically more forgiving on the first interaction.

# Useful token under 1 second for simple prompts