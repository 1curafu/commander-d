## Jira story

<!-- Example: CD-5, https://commander24c.atlassian.net/browse/CD-5 -->

## What changed

<!-- Two or three sentences. -->

## How I tested it

<!-- Which commands did you run? Tested on Windows? -->

## Compared with the real cmd.exe

<!-- Paste the output of the real command and of ours, or add a screenshot. -->

## Checklist

- [ ] The branch name is `feature/CD-<number>-<short-name>`
- [ ] The code builds and the shell starts
- [ ] Every acceptance criterion of the Jira story is fulfilled
- [ ] Output and error messages match the real `cmd.exe`
- [ ] The command extends `AbstractCommand` and is created by `CommandFactory`
- [ ] File and console access only through `IFileSystemService` and `IConsoleService`
- [ ] Paths use `Path.Combine`, no Windows-only features
- [ ] No unrelated changes, no leftover debug code
- [ ] Reviewer is set (Mykhailo and Oliver)
