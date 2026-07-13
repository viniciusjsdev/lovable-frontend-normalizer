# Install Skills Workflow

Use this workflow to install skills from this repository into the local Codex user skill directory.

```powershell
$codexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$skillsHome = Join-Path $codexHome "skills"

Get-ChildItem .\skills -Directory | ForEach-Object {
  Copy-Item -Recurse $_.FullName (Join-Path $skillsHome $_.Name) -Force
}
```

Install a single skill:

```powershell
$codexHome = if ($env:CODEX_HOME) { $env:CODEX_HOME } else { Join-Path $env:USERPROFILE ".codex" }
$skillName = "<skill-name>"
Copy-Item -Recurse (Join-Path ".\skills" $skillName) (Join-Path $codexHome "skills\$skillName") -Force
```
