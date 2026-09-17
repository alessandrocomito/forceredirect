# ForceRedirect
Version 3.6 (2026-09-17)  

**ForceRedirect** is a small, portable, native 64-bit C++ Windows utility that allows you to capture the console output of command-line programs that cannot be captured correctly using standard `>` redirection.

It uses the Windows ConPTY (Pseudo Console) API, available on Windows 10 version 1809 or later and Windows 11, to capture console output from programs that do not behave correctly with conventional stdout redirection.

It is built with Microsoft's Visual C++ (cl.exe) compiler for the x64 target.

Normally, console output can be saved with:

    program.exe > output.txt

However, some programs display output in the console but do not expose their output correctly through standard redirection.

ForceRedirect provides a simple solution:

    ForceRedirect.exe program.exe output.txt

The target program runs normally while its console output is captured into the specified file.

---

## Features

- Captures console output using Windows ConPTY
- Configurable buffer height with `-rows=N` parameter (default: 30000, max: 32767)
- Optional preservation of ANSI/VT color sequences with the `-color` parameter (default: off)
- Works with existing Windows command-line programs
- Supports the target program's command-line arguments
- Handles large amounts of console output
- Preserves Unicode characters
- Handles console line breaks correctly
- Writes the output file progressively while the target program is running
- Updates the output file approximately once per second
- Portable and standalone
- No installation required
- Works from Command Prompt, batch files and PowerShell
- Suitable for automated workflows
- Does not require modifications to the target program

---

## Download

### ForceRedirect.exe (57.5 KB (58880 byte))

**[Preview / Download Zip](https://bit.ly/4x9XNpf)** (25.8 KB (26.473 byte))

Opens the Google Drive preview page, where you can inspect the file before downloading it.

**[Direct Download Zip](https://bit.ly/4cuAEW0)**

Downloads `ForceRedirect.exe` directly from Google Drive without opening the preview page.

No installer is required.

---

## Requirements

- Windows 10 x64 version 1809 or later
- A Windows command-line program whose console output you want to capture

ForceRedirect is a standalone portable executable.

---

## Command-Line Syntax

    ForceRedirect.exe [-rows=N] [-color] <target.exe> [target arguments...] <output_file>

The optional `-rows=N` and `-color` parameters may be used independently or together.

### Parameters

- **`-rows=N`** *(Optional)*: Specifies the console buffer height (number of rows) allocated for ConPTY.
  - **Default:** `30000` if omitted.
  - **Maximum Limit:** `32767` (hard limit imposed by the Windows API `SHORT` coordinate structure).
  - Any value greater than `32767` is automatically clamped to `32767`.

- **`-color`** *(Optional)*: Preserves ANSI/VT color escape sequences in the output file.
  - **Default:** disabled.
  - If omitted, ANSI/VT color and terminal styling sequences are removed and the output contains clean plain text.
  - If specified, color escape sequences are preserved in the captured output.

- **`<target.exe>`**: The executable to launch.

- **`[target arguments...]`** *(Optional)*: Any arguments passed directly to the target executable.

- **`<output_file>`**: The output file where the captured console output will be written.

ForceRedirect does not otherwise interpret or modify the target executable's arguments. Their number and meaning depend entirely on the target executable.

---

## Examples

### Basic Usage

The simplest form uses the default settings:

    ForceRedirect.exe flad_cli.exe file.flac output.txt

This is equivalent to:

    ForceRedirect.exe -rows=30000 flad_cli.exe file.flac output.txt

The default configuration is therefore:

- Buffer: `30000` rows
- Color: disabled

### Using `-rows=N`

Specifying 1,000 buffer rows:

    ForceRedirect.exe -rows=1000 flad_cli.exe file.flac output.txt

### Using `-color`

Preserving ANSI/VT color sequences:

    ForceRedirect.exe -color flad_cli.exe file.flac output.txt

When `-color` is used without `-rows=`, the buffer size remains `30000` rows.

### Using both optional parameters

    ForceRedirect.exe -rows=100 -color flad_cli.exe file.flac output.txt

The order of `-rows=N` and `-color` is not significant:

    ForceRedirect.exe -color -rows=100 flad_cli.exe file.flac output.txt

### Passing additional target arguments

    ForceRedirect.exe program.exe --input input.flac --verbose output.txt

In this example:

- `program.exe` is the target executable.
- `--input input.flac --verbose` are arguments belonging to `program.exe`.
- `output.txt` is the output file created by ForceRedirect.

---

## FLAD

`flad_cli.exe` is a practical example of a command-line program whose console output cannot be reliably captured using normal `>` redirection.

Without ForceRedirect:

    flad_cli.exe input.flac > output.txt

the complete console output is not captured as expected.

With ForceRedirect:

    ForceRedirect.exe flad_cli.exe input.flac output.txt

the console output is captured in `output.txt`.

The output file is also updated progressively while FLAD is running, making it possible for another program or script to inspect the captured output before FLAD has finished.

This is the primary type of situation ForceRedirect is designed to solve.

---

## tree.com — High-Volume Output Test

Windows `tree.com` supports normal `>` redirection correctly, so it does **not** require ForceRedirect.

It is nevertheless useful as a high-volume output test because it can produce hundreds or thousands of lines when scanning a directory tree.

Standard redirection:

    tree.com C:\Windows > tree.txt

Through ForceRedirect:

    ForceRedirect.exe tree.com C:\Windows tree.txt

This provides a convenient way to test output integrity with a large amount of console data.

The test can verify that:

- large amounts of output are captured correctly;
- output is not limited by the visible console scrollback;
- Unicode characters are preserved;
- line breaks are handled correctly;
- unexpected blank lines are not introduced.

---

## Output Handling

ForceRedirect captures the console stream directly through ConPTY rather than relying on the visible scrollback history of the Command Prompt or PowerShell window.

The captured output is written to the specified output file and is updated progressively while the target program is running.

The file is refreshed approximately once per second and receives a final update when the target program terminates.

This allows another process, script or user to inspect the captured output while the target program is still running.

The output file is saved as UTF-8 text, allowing Unicode characters to be preserved.

By default, ANSI/VT color and terminal control sequences are removed, producing clean plain-text output suitable for text editors and automated processing.

When `-color` is specified, ANSI/VT color escape sequences are preserved in the output file.

If the specified output file already exists, it is overwritten.

---

## Batch Files

ForceRedirect can be used directly from `.bat` or `.cmd` files.

Example:

    @echo off
    ForceRedirect.exe flad_cli.exe input.flac output.txt

Using both optional parameters:

    @echo off
    ForceRedirect.exe -rows=100 -color flad_cli.exe input.flac output.txt

Another example:

    @echo off
    ForceRedirect.exe tree.com C:\Windows tree.txt

This makes it possible to integrate programs with difficult-to-capture console output into automated workflows.

---

## PowerShell

ForceRedirect can also be used from PowerShell:

    .\ForceRedirect.exe flad_cli.exe input.flac output.txt

The resulting file can then be processed normally.

For example:

    $result = Get-Content .\output.txt

---

## Portable

ForceRedirect is completely portable.

There is no installer and no configuration is required.

Simply place `ForceRedirect.exe` wherever you need it.

Example:

    Tools\
    ├── ForceRedirect.exe
    └── flad_cli.exe

Then run:

    ForceRedirect.exe flad_cli.exe input.flac output.txt

---

## Typical Use Cases

### Capture difficult console output

Save console output that cannot be reliably captured using normal `>` redirection.

### Monitor output during execution

Allow scripts or other applications to inspect the captured output while the target program is still running.

### Integrate existing command-line tools

Use an existing executable in an automated workflow without modifying the original program.

### Batch processing

Capture output to files that can subsequently be processed by batch files or other utilities.

### Preserve colored console output

Use `-color` when ANSI/VT color sequences need to be retained in the captured output.

### PowerShell automation

Capture command-line output and process it with PowerShell.

### High-volume output testing

Test console-output capture with programs such as `tree.com` that can generate hundreds or thousands of lines.

---

## Limitations

ForceRedirect is intended for Windows command-line programs that produce text output in a console.

It is not intended for graphical applications or programs whose output is primarily graphical rather than textual.

The behavior of individual programs may vary depending on how they produce their console output.

The `-color` option preserves ANSI/VT escape sequences in the output file; whether those sequences are displayed as actual colors depends on the application used to open or process the file.

---

## Quick Reference

### Syntax

    ForceRedirect.exe [-rows=N] [-color] <target.exe> [target arguments...] <output_file>

### Optional parameters

- **`-rows=N`**: Number of buffer rows for ConPTY. Default: `30000`. Maximum: `32767`.
- **`-color`**: Preserve ANSI/VT color escape sequences. Default: disabled.

### Defaults

    ForceRedirect.exe <target.exe> [target arguments...] <output_file>

is equivalent to using:

    -rows=30000

with color disabled.

### Target executable

The executable to launch.

### Intermediate parameters

Arguments passed directly to the target executable.

These parameters must be supported by the target executable.

---

## Project Purpose

ForceRedirect has one simple purpose:

> **Capture console output that cannot be reliably captured using normal `>` redirection.**

It is designed to be small, portable, and easy to use.

---

## License

Freeware.

Copyright (c) 2026 Alessandro Comito.
```
