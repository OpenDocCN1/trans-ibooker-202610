stream “I’ll cover: 1) X, 2) Y, 3) Z,” and then fill in each section progressively. This provides orientation and context.

# Guarded tool use

When the AI needs to call a tool or query a database (which are common in agentic workflows), run the tool call asynchronously but reveal its status quickly. Show a placeholder: “Fetching data from database…” or “Running calculation…” If the tool is slow, give the user a retry or cancel option. When the results arrive, replace the placeholder smoothly. This keeps the user informed and in control, even when backend operations take time.

# Pipelined RAG

In RAG systems, you first retrieve relevant documents or data then generate a response based on them. Instead of waiting for retrieval to complete before showing anything, stream the retrieved sources first as they arrive (“Found 3 relevant documents: A, B, C…”), then stream the synthesized answer below. Finally, highlight or link to specific parts of the sources as the answer references them. This gives users confidence in the data and lets them explore sources while the answer is still being generated.

# Streaming diffs for code or docs

When the AI is generating or modifying code or documents, render a stable base version, then apply diffs in place. For example, show the original code with a subtle highlight around the area being changed, then animate the change smoothly. This avoids the entire code block rerendering and jumping around, which would cause scroll jumps and CLS. Users can follow along with the changes without losing their place.