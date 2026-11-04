Reproduced on `main` at `f6e1d5f`, Windows 11 Pro 10.0.26200, Node v24.13.1, Pi 1.3.0 (via `npm ci`).

**Result:** `ToolBridge._registerTool` does not pass the structured `annotations` to `pi.registerTool`. The hints reach the model only as description text.

**Steps**
1. `git checkout f6e1d5f && npm ci`
2. Add a temporary `tests/zz-repro-253.test.ts` (below), using a mock Pi and a mock MCP client whose `tools/list` returns one tool with `annotations: { title: "Delete", destructiveHint: true, readOnlyHint: false }`.
3. `node --import tsx/esm --test tests/zz-repro-253.test.ts`

**Actual** (exit code 1)
```
description     : Delete a record [⚠️ destructive]
annotations     : undefined
AssertionError: Expected values to be strictly deep-equal:
+ undefined
- { destructiveHint: true, readOnlyHint: false }
```

**Expected:** the registered tool also carries `annotations: { destructiveHint: true, readOnlyHint: false }` (`title` is already used as `label`).

Pi 1.3.0 does declare the field: `ToolAnnotations` at L400 and `annotations?: ToolAnnotations` at L492 in `types.d.ts`.

<details><summary>Repro test</summary>

```ts
import { describe, it } from "node:test";
import assert from "node:assert/strict";
import type { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { ToolBridge } from "../src/tool-bridge.js";

describe("repro #253", () => {
  it("registerTool receives the structured annotations from tools/list", async () => {
    const registered: Array<Record<string, unknown>> = [];
    let active: string[] = [];
    const mockPi = {
      registerTool: (t: unknown) => registered.push(t as Record<string, unknown>),
      getActiveTools: () => active,
      setActiveTools: (names: string[]) => { active = names; },
    };
    const mockClient = {
      request: async () => ({
        tools: [{
          name: "delete_record",
          description: "Delete a record",
          inputSchema: { type: "object", properties: {}, required: [] },
          annotations: { title: "Delete", destructiveHint: true, readOnlyHint: false },
        }],
      }),
    } as unknown as Client;

    const bridge = new ToolBridge({ toolPrefix: "mcp", requestTimeoutMs: 30000, maxRetries: 5 }, mockPi);
    await bridge.refreshTools("myserver", mockClient);

    const tool = registered[0]!;
    console.log("registered keys :", Object.keys(tool).join(", "));
    console.log("description     :", tool["description"]);
    console.log("annotations     :", JSON.stringify(tool["annotations"]));
    assert.deepEqual(tool["annotations"], { destructiveHint: true, readOnlyHint: false });
  });
});
```
</details>

**Limits:** this is a unit-level repro of what the bridge hands to `registerTool`. I haven't checked how a real permission extension reacts. Existing tests only assert the description text, which is why this wasn't caught.

I used Claude Code (Sonnet 5.5) to read the code, draft the throwaway test and write this up. The output above is from a real run on my machine, and I reviewed it before posting.
