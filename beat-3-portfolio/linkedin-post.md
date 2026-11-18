It’s official: my first ever open-source contribution just got merged! 🎉🎉 It landed in pi-mcp-extension, an MCP extension for Pi — a minimal coding agent harness.

I'd been using AI agents for months before I contributed to the tools behind them. That changed in CodePath's AI301 capstone, where the assignment was simple yet intimidating: pick a real open-source repo and ship something to it.

The problem: MCP tools carry safety hints, like “this one is destructive” or “this one only reads.” The extension was writing those hints into the tool’s description text but never passing them to Pi as structured data, so permission extensions saw nothing at all.

My PR passes them through, so those extensions can now check before an agent does something irreversible.

The fix itself was small, but the work around it was the real deal:

- Reproducing the issue with a failing test first
- Following the repo's contribution guidelines
- Communicating with the maintainer before writing any code 

I'd like to thank Ichsan Rahardianto for maintaining this very exciting project and reviewing my PR, and the CodePath team for being ever-so supportive in helping me and my peers take our first steps in open-source.

https://lnkd.in/g4Y-5fHT

#OpenSource #MCP #AIAgents #CodePath