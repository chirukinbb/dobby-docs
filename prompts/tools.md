### Available Tools

1. **executePythonCode**
    - **Описание**: Execute Python code directly via Chaquopy embedded interpreter.
    - **Аргументы**: `code` (string).
    - **Возвращает**: ScriptExecutionResult
```kotlin
data class ScriptExecutionResult(
    val success: Boolean,
    val output: String?,
    val error: String?
)
   ```
2. **writeFile**
    - **Описание**: Write content to file.
    - **Аргументы**:
        - `filePath` (string) – path to the file.
        - `content` (string) – text to write.
        - `append` (boolean, optional, default = false) – if true, appends to the end of the file, otherwise overwrites.
    - **Возвращает**: FileOperationResult (success/error)

3. **listFiles**
    - **Описание**: List files in directory.
    - **Аргументы**:
        - `directoryPath` (string) – path to the directory.
        - `pattern` (string, optional, default = "*") – filter pattern (not used in current implementation).
    - **Возвращает**: FileListResult
   ```kotlin
   data class FileListResult(
       val success: Boolean,       // operation success status
       val files: List<String>,    // list of absolute file paths
       val error: String?          // error message
   )
   ```
4. **deleteFile**
    - **Описание**: Delete a file.
    - **Аргументы**: `filePath` (string) – path to the file.
    - **Возвращает**: FileOperationResult (success/error)

5. **executeShellCommand**
    - **Описание**: Execute shell command synchronously.
    - **Аргументы**: `command` (string) – command to execute.
    - **Возвращает**: ScriptExecutionResult
   ```kotlin
   data class ScriptExecutionResult(
       val success: Boolean,   // execution success (exit code == 0)
       val output: String?,    // command stdout
       val error: String?      // stderr or error message
   )
   ```
6. **executeCommandAsync**
    - **Описание**: Execute command asynchronously with callback.
    - **Аргументы**:
        - `command` (string) – command to execute.
        - `callback` (function) – function that accepts ScriptExecutionResult and is called upon completion.
    - **Возвращает**: void (asynchronous call)

7. **executePythonCode**
    - **Описание**: Execute Python code directly via Chaquopy embedded interpreter.
    - **Аргументы**: `code` (string) – Python code to execute.
    - **Возвращает**: ScriptExecutionResult

8. **executePythonScript**
    - **Описание**: Execute a Python script file via Chaquopy embedded interpreter.
    - **Аргументы**:
        - `scriptPath` (string) – path to the .py file.
        - `args` (list of strings, optional, default = null) – command-line arguments for the script.
    - **Возвращает**: ScriptExecutionResult

9. **executeCommand (overloaded)**
    - **Описание**: Execute command via Chaquopy. Detects Python scripts and routes accordingly. Falls back to shell execution if Chaquopy is unavailable.
    - **Аргументы**: `command` (string) – command or script path.
    - **Возвращает**: ScriptExecutionResult

10. **enqueueTaskWork**
    - **Описание**: Enqueue a task with WorkManager for background execution. Task persistence is handled separately via Room.
    - **Аргументы**: `task` (ScheduledTask) – task object.
    ```kotlin
    data class ScheduledTask(
        val id: String,            // unique identifier
        val name: String,          // task name
        val command: String,       // command to execute
        val scheduleType: String,  // schedule type ("once", "daily", "weekly", "interval")
        val scheduleValue: String, // schedule value (e.g., "300" for interval in seconds)
        val enabled: Boolean = true
    )
    ```
    - **Возвращает**: Boolean – true if the task was successfully queued, false otherwise.

# File System Rules (Android-specific)

### 1. Priority of Tools
- **Shell-First Approach:** Always prioritize `executeShellCommand` if the task can be accomplished using standard utilities (`mkdir`, `mv`, `cp`, `rm`, `ls`, `touch`).
- **Python Usage:** Use **Python** (with `dobby_utils`) only for complex logic, such as file content analysis, conditional loops, or handling ambiguous permissions/encodings.

### 2. Command Granularity & Debugging
- **Avoid Long Chains:** Do not combine logically distinct steps into a single long string using `&&` if it makes identifying the point of failure difficult.
- **Granular Execution:** Break operations into separate `executeShellCommand` calls. This ensures that if an error occurs, the logs clearly show exactly which command failed.

### 3. Path Absolute Integrity
- **Strict Absolute Paths:** ALWAYS use absolute paths starting with `/sdcard/`.
- **No Relative Paths:** Never use relative paths like `Archive/` or `./`.

### 4. Validation & Safety
- **Incremental Checks:** When using `&&` within a single call for atomic operations (e.g., `mkdir -p ... && touch ...`), ensure the critical step is verified before proceeding.
- **Verification:** After creating a directory or moving a critical file, use a subsequent call to verify its existence (e.g., `ls` or `[ -d path ]`) to guarantee system integrity before executing dependent logic.

