# Commander D

A Windows-style command shell for the console, written in C# on .NET 10.
School project of **Team D**, built in three Scrum sprints. The backlog lives in
Jira (project **Commander D**, key `CD`).

The shell imitates the Windows command prompt (`cmd.exe`): it shows the current
directory as a prompt, reads a command, runs it and shows the result.

## Team

| Name | Role |
|---|---|
| Mykhailo | Product Owner, developer |
| Oliver | Scrum Master, developer |
| Noel O. | Developer |
| Florian | Developer |
| Mario Sabbatella | Stakeholder |

## Tools

| What | Choice |
|---|---|
| Language | C# |
| Framework | .NET 10 |
| IDE | JetBrains Rider |
| Version control | Git, GitHub |
| Backlog | Jira |
| Target platform | **Windows** |

Four of us develop on Windows, one on macOS. Keep the code platform-neutral so it
builds on both:

- Build paths with `Path.Combine(...)`. Never write `\` or `/` into a path by hand.
- Do not use Windows-only features (registry, Windows Forms, calling `cmd.exe`).
- Test your work on Windows before every sprint review.

## Getting started

1. Install the **.NET 10 SDK**. Check it with `dotnet --version`, the output must
   start with `10.`
2. Use a recent Rider version that supports .NET 10 (update it if Rider does not
   offer `net10.0` for new projects).
3. Clone the repository and open the folder in Rider.
4. Build and run (once the solution exists):

   ```
   dotnet build
   dotnet run --project <path to the shell project>
   ```

`global.json` pins the SDK to 10.0, so everyone builds with the same major version.

## Architecture

The design uses two patterns: the **Command Pattern** and a **Simple Factory**.
The current class diagram is in [`docs/architecture.svg`](docs/architecture.svg).

| Class | Job |
|---|---|
| `CommandShell` | The loop: shows the prompt, reads input, runs the command (invoker) |
| `CommandParser` | Splits the input into a `CommandContext` |
| `CommandContext` | Command name, arguments and the raw input |
| `CommandFactory` | Simple Factory: creates the matching `ICommand` by name |
| `ICommand` / `AbstractCommand` | The common command interface and its base class |
| One class per command | `DirCommand`, `CdCommand`, `MkDirCommand`, ... |
| `CommandResult` | Success, output message and `ShouldExit` |
| `IFileSystemService`, `IConsoleService` | All file and console access (receivers) |

Command names are compared ignoring case, like in Windows (`dir`, `DIR`, `Dir`).

## Behaviour: same as the real `cmd.exe`

The goal is that Commander D looks and behaves like the Windows command prompt.
When you work on a story, run the real command in `cmd.exe` and compare:

- **Output format:** same layout, spacing and wording (for example the `DIR`
  header, the `<DIR>` column and the footer with file counts and free bytes).
- **Error messages:** the exact text of Windows, for example
  `The system cannot find the file specified.` (TYPE) or
  `Cannot access file <full path>` (MORE).
- **Regional format:** dates and times follow the Windows regional settings.
- **Keys:** interactive commands such as `MORE` react to the same keys as Windows.

The acceptance criteria of CD-2 (DIR), CD-4 (TYPE) and CD-6 (MORE) already contain
this rule. Add it to other stories when we refine them. Keep this in the output
code only, so the logic stays platform-neutral.

## Sprint plan

| Sprint | Dates | Planned |
|---|---|---|
| 1 | 27.10. to 30.10.2026 | CD-14 foundation, CD-9 prompt, CD-1 EXIT, CD-8 VER, CD-10 CLS, CD-5 CD, CD-2 DIR, CD-3 MKDIR, CD-12 SET COLOR (20 SP) |
| 2 | 03.11. to 06.11.2026 | CD-13 DEL, CD-7 REN, CD-4 TYPE, CD-6 MORE (19 SP) |
| 3 | 10.11.2026 | CD-11 MOVE (13 SP) |

Sprints 2 and 3 are a first guess. We adjust them after Sprint 1 with the real
velocity (achieved SP divided by the total capacity).

## Working with Git

Short version: one branch per Jira story (`feature/CD-<number>-<short-name>`),
merged into `main` with a pull request. Mykhailo and Oliver reviews every pull request. Nobody pushes to `main` directly. Details are in
[`docs/git-workflow.md`](docs/git-workflow.md).
