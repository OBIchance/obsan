+++
title = 'Htb'
date = 2024-08-09T19:47:07-05:00
draft = false
author = "Obsan Muzemil"
updated = 2024-08-09
+++

## Homelab Project
```bash
# List all files in a directory
ls -la

# Change directory
cd /path/to/directory

# Display the current directory
pwd


### 3. **Code Block for PowerShell Commands**

Similarly, for PowerShell commands, specify the language as `powershell`:

```markdown
```powershell
# Get the list of processes
Get-Process

# List files in a directory
Get-ChildItem -Path C:\

# Display system information
Get-ComputerInfo


### 4. **Using Hugo's Highlight Shortcode**

You can also use Hugo's internal highlight shortcode to add these command blocks, which allows you to include additional options like line numbers:

#### Linux Commands:

```markdown
{{< highlight bash >}}
# List all files in a directory
ls -la

# Change directory
cd /path/to/directory

# Display the current directory
pwd
{{< /highlight >}}
