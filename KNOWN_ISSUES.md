Below is an updated version of the bug documentation that incorporates the new error details and recommendations. You can add this to your dedicated **BUGS.md** file or a "Known Issues" section in your README.

---

# Known Issues

## Bug: Threads Stuck in `requires_action` During Parallel Tool Calls

**Summary:**  
When a single message contains multiple instructions that trigger parallel tool calls, some threads can get stuck in a `requires_action` state. This may lead an unhandled error.

**Reproduction Steps:**  
Send a message containing multiple instructions in a single input. For example:

```plaintext
What's my company's remote work policy? Check tomorrow's weather in Redmond, do I need a jacket? What's the latest news about Microsoft? Send a recap in my inbox.
```

**Observed Behavior:**  
- The system processes the instructions in parallel.
- Some threads become stuck in the `requires_action` state, leading to delays or incomplete processing.
- When a new message is attempted during an active run, the following error occurs:

  ```plaintext
  azure.core.exceptions.HttpResponseError: (None) Can't add messages to thread_RSMZo0dlUVQtQevUe6Y2z18D while a run run_h10zMlNLNk4PQB8BIXVg8tKM is active.
  ```

**Expected Behavior:**  
- All instructions should be processed concurrently without interference.
- No thread should remain indefinitely in the `requires_action` state.
- New messages should either queue or start a new thread gracefully, without raising unhandled errors.

**Workaround:**  
- **Break Down Tasks:** Split your instructions into separate messages rather than combining them into one.  
- **Clear Conversation:** If the error occurs, clear (🗑️) the current conversation to start a new thread before attempting to send new messages.

**Status:**  
- This issue is under investigation. Contributions to resolve the bug are welcome.

**Notes for Contributors:**  
- Investigate the parallel processing and state management code for potential race conditions or deadlocks causing threads to remain in the `requires_action` state.
- Review how new messages are queued or added during an active run and consider mechanisms (such as adaptively disabling parallel tool calls when multiple instructions are submitted by a user) to either queue them or reset the state to prevent the unhandled error.
- When submitting a pull request, reference this issue and update the status accordingly.