# ForceRedirect

**ForceRedirect** is a small, portable, native 64-bit C++ Windows utility that allows you to capture the console output of command-line programs that cannot be captured correctly using standard > redirection.

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
- Works with existing Windows command-line programs
- Supports the target program's command-line arguments
- Handles large amounts of console output
- Preserves Unicode characters
- Handles console line breaks correctly
- Portable and standalone
- No installation required
- Works from Command Prompt, batch files and PowerShell
- Suitable for automated workflows
- Does not require modifications to the target program

---

## Download

### ForceRedirect.exe (39.0 KB (39.936 byte))

**[Preview / Download Zip](https://bit.ly/4x9XNpf)** (16.4 KB (16.820 byte))

Opens the Google Drive preview page, where you can inspect the file before downloading it.

**[Direct Download Zip](https://bit.ly/4cuAEW0)**

Downloads `ForceRedirect.exe` directly from Google Drive without opening the preview page.

No installer is required.

---

## Requirements

- Windows 10 version 1809 or later
- A Windows command-line program whose console output you want to capture

ForceRedirect is a standalone portable executable.

---

## Command-Line Syntax

    ForceRedirect.exe <target.exe> [target arguments...] <output_file>

ForceRedirect requires at least two parameters:

- The **first parameter** is the executable to launch.
- The **last parameter** is the output file.
- Any parameters between the first and last parameters are passed directly to the target executable.

ForceRedirect does not interpret or modify the intermediate parameters. Their number and meaning depend entirely on the target executable.

### Example

    ForceRedirect.exe program.exe --input input.flac --verbose output.txt

In this example:

- `program.exe` is the target executable.
- `--input input.flac --verbose` are arguments belonging to `program.exe`.
- `output.txt` is the output file created by ForceRedirect.

---

## Examples

### FLAD

`flad_cli.exe` is a practical example of a command-line program whose console output cannot be reliably captured using normal `>` redirection.

Without ForceRedirect:

    flad_cli.exe input.flac > output.txt

the complete console output is not captured as expected.

With ForceRedirect:

    ForceRedirect.exe flad_cli.exe input.flac output.txt

the console output is captured in `output.txt`.

This is the primary type of situation ForceRedirect is designed to solve.

### tree.com — High-Volume Output Test

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

The captured output is written directly to the specified output file.

This allows programs producing hundreds or thousands of lines to be captured without depending on the size of the visible console buffer.

The output file is saved as UTF-8 text, allowing Unicode characters to be preserved.

If the specified output file already exists, it is overwritten.

---

## Batch Files

ForceRedirect can be used directly from `.bat` or `.cmd` files.

Example:

    @echo off
    ForceRedirect.exe flad_cli.exe input.flac output.txt

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

### Integrate existing command-line tools

Use an existing executable in an automated workflow without modifying the original program.

### Batch processing

Capture output to files that can subsequently be processed by batch files or other utilities.

### PowerShell automation

Capture command-line output and process it with PowerShell.

### High-volume output testing

Test console-output capture with programs such as `tree.com` that can generate hundreds or thousands of lines.

---

## Limitations

ForceRedirect is intended for Windows command-line programs that produce text output in a console.

It is not intended for graphical applications or programs whose output is primarily graphical rather than textual.

The behavior of individual programs may vary depending on how they produce their console output.

---

## Quick Reference

### Syntax

    ForceRedirect.exe <target.exe> [target arguments...] <output_file>

### First parameter

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
