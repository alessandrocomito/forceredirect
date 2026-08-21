# ForceRedirect

**ForceRedirect** is a small, portable Windows utility that allows you to capture the console output of command-line programs that cannot be captured correctly using standard `>` redirection.

Normally, you can save the output of a command-line program to a text file with:

    program.exe > output.txt

However, some programs display output in the console but do not allow that output to be captured correctly this way.

ForceRedirect provides a simple solution:

    ForceRedirect.exe program.exe output.txt

The program runs normally, while its console output is captured into the specified output file.

---

## Why use ForceRedirect?

ForceRedirect is useful when:

- a command-line program displays output correctly on screen;
- using `>` does not capture all of that output;
- you need to save the complete console output to a text file;
- you need to process the output automatically with another program or script.

You do not need to modify the original program.

---

## Features

- Simple command-line interface
- Works with existing Windows command-line programs
- Supports the target program's command-line arguments
- Captures console output directly to a text file
- Handles large amounts of console output
- Preserves Unicode characters
- Handles normal line breaks correctly
- Portable
- No installation required
- Standalone executable
- Can be used from Command Prompt
- Can be used from batch files
- Can be used from PowerShell
- Suitable for automated workflows

---

## Download

### ForceRedirect.exe

**[Preview / Download](https://bit.ly/4x9XNpf)**

Opens the Google Drive preview page, where you can inspect the file before downloading it.

**[Direct Download](https://bit.ly/4cuAEW0)**

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

ForceRedirect requires at least **two parameters**:

- The **first parameter** is always the executable to launch.
- The **last parameter** is always the output file.
- Any parameters between the first and last parameters are passed directly to the target executable.

The intermediate parameters must be valid parameters supported by the target executable.

**ForceRedirect does not interpret or modify these parameters.**

### Example

    ForceRedirect.exe program.exe --input input.flac --verbose output.txt

In this example:

- `program.exe` is the target executable.
- `--input input.flac --verbose` are parameters belonging to `program.exe`.
- `output.txt` is the output file created by ForceRedirect.

The number and meaning of the intermediate parameters depend entirely on the target executable.

---

# Examples

## FLAD CLI

`flad_cli.exe` is a practical example of a command-line program whose console output cannot be reliably captured using normal `>` redirection.

Without ForceRedirect:

    flad_cli.exe input.flac > output.txt

the complete console output is not captured as expected.

With ForceRedirect:

    ForceRedirect.exe flad_cli.exe input.flac output.txt

the console output can be captured in `output.txt`.

This is the primary type of situation ForceRedirect is designed to solve.

---

## tree.com — High-Volume Output Test

Windows `tree.com` is useful for a different reason.

It already supports normal `>` redirection correctly, so it is **not** an example of a program that requires ForceRedirect.

Instead, `tree.com` is useful as a high-volume output test because it can produce hundreds or thousands of lines when scanning a directory tree.

For example, standard redirection works correctly:

    tree.com C:\Windows > tree.txt

The same command can be run through ForceRedirect:

    ForceRedirect.exe tree.com C:\Windows tree.txt

This provides a convenient way to test ForceRedirect with a large amount of console output.

The test can be used to verify that:

- large amounts of output are captured correctly;
- output is not limited to a small number of lines;
- Unicode characters are preserved;
- line breaks are handled correctly;
- no unexpected blank lines are introduced.

The purpose of the `tree.com` test is therefore **output-volume and output-integrity testing**, not demonstrating a limitation of standard `>` redirection.

---

# Output Handling

ForceRedirect is designed to handle large amounts of console output without relying on the visible scrollback history of the Command Prompt or PowerShell window used to launch it.

The captured output is written directly to the specified output file.

This makes ForceRedirect suitable for command-line programs that produce hundreds or thousands of lines of output.

The output file is saved as UTF-8 text, allowing Unicode characters to be preserved.

---

# Command-Line Examples

### Capture a program's output

    ForceRedirect.exe program.exe output.txt

### Capture a program with one argument

    ForceRedirect.exe program.exe input.dat output.txt

### Capture a program with multiple arguments

    ForceRedirect.exe program.exe --input input.dat --verbose output.txt

### FLAD CLI

    ForceRedirect.exe flad_cli.exe input.flac output.txt

### Large output test with `tree.com`

    ForceRedirect.exe tree.com C:\Windows tree.txt

---

# Batch Files

ForceRedirect can easily be used in `.bat` or `.cmd` files.

Example:

    @echo off
    ForceRedirect.exe flad_cli.exe input.flac output.txt

Another example using `tree.com`:

    @echo off
    ForceRedirect.exe tree.com C:\Windows tree.txt

This makes it possible to integrate programs with difficult-to-capture console output into automated workflows.

---

# PowerShell

ForceRedirect can also be used from PowerShell:

    .\ForceRedirect.exe flad_cli.exe input.flac output.txt

The resulting text file can then be processed normally by PowerShell or another application.

For example:

    $result = Get-Content .\output.txt

---

# Portable

ForceRedirect is completely portable.

There is no installer and no configuration is required.

Simply place `ForceRedirect.exe` wherever you need it and run it from the command line.

For example:

    Tools\
    ├── ForceRedirect.exe
    └── flad_cli.exe

You can then run:

    ForceRedirect.exe flad_cli.exe input.flac output.txt

---

# Output File

The output file is specified as the **last parameter**.

If the file already exists, it is overwritten.

For example:

    ForceRedirect.exe flad_cli.exe input.flac result.txt

creates or replaces:

    result.txt

The captured output is saved as UTF-8 text.

---

# Typical Use Cases

### Capture console output

Save console output that cannot be reliably captured using normal `>` redirection.

### Integrate existing command-line tools

Use an existing executable in an automated workflow without modifying the original program.

### Batch processing

Capture output to files that can subsequently be processed by batch files or other utilities.

### PowerShell automation

Capture command-line output and process it with PowerShell.

### High-volume output testing

Use programs such as `tree.com` to test ForceRedirect with hundreds or thousands of lines of console output.

---

# Limitations

ForceRedirect is intended for Windows command-line programs that produce text output in a console.

It is not intended for graphical applications or programs whose output is primarily graphical rather than textual.

The behavior of individual programs may vary depending on how they produce their console output.

---

# Quick Reference

### Syntax

    ForceRedirect.exe <target.exe> [target arguments...] <output_file>

### First parameter

The executable to launch.

### Intermediate parameters

Arguments passed directly to the target executable.

These parameters must be supported by the target executable.

### Last parameter

The output file created by ForceRedirect.

### FLAD CLI

    ForceRedirect.exe flad_cli.exe input.flac output.txt

### `tree.com` high-volume test

    ForceRedirect.exe tree.com C:\Windows tree.txt

---

# Project Purpose

ForceRedirect has one simple purpose:

> **Capture console output that cannot be reliably captured using normal `>` redirection.**

It is designed to be small, portable, and easy to use.

---

# License

Freeware.

Copyright (c) 2026 Alessandro Comito.
