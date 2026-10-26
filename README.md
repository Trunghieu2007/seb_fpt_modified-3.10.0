# seb_fpt_modified-3.10.0

Summary of the custom FPT Safe Exam Browser 3.10.0 build used in my SEB security research, and the matching installer.

This is a custom FPT Windows x64 package, and `Application/` is the working copy of it with modified restriction behaviour. It documents a finding, it is not a supported product.

## Repository layout

Two folders, everything else is metadata.

| Path | What it holds |
| --- | --- |
| `Application/` | The runnable program: `SafeExamBrowser.exe`, `SafeExamBrowser.Client.exe`, SEB and CefSharp assemblies, Chromium resources, plus the `Configuration`, `Reset` and `Service` component folders |
| `Installation/` | `SEB_3.10.0.794_AutoSettings.msi`, the Windows Installer package for the same build, 311 MB |
| `.gitattributes` | Declares that `.dll`, `.exe` and `.msi` are stored with Git LFS |
| `README.md` | This file |

## Versions

| | Version | Source |
| --- | --- | --- |
| Build in this repository | 3.10.0.794 | Custom FPT package |
| Replacement build | 3.12 | Custom FPT package, dated 15 November 2026 |

The 3.10.0.794 and 3.12 numbers belong to the custom packages distributed in this environment, not to the upstream release line. Upstream has nothing newer than 3.10.2, so version strings here cannot be compared with the official ones.

The MSI is the part that matters. The `Application` folder is a working copy; the MSI installs the same build into `C:\Program Files (x86)\`, registers the service component and creates the Start Menu entries.

## Installation

This build only works on 3.10.0.794. The 3.12 package replaces it and removes the behaviour being studied.

1. Check what is installed: Settings, Apps, look for "Safe Exam Browser" and note the version.
2. If a 3.12 build is present, uninstall it first and let the removal finish. Leaving both installed keeps the newer service component registered, and that component is what the older build talks to.
3. Install `Installation/SEB_3.10.0.794_AutoSettings.msi` by double clicking it.
4. Confirm the registered version reads 3.10.0.794 before starting a session.

The uninstall entries created by that package appear as "Safe Exam Browser (x64)" and "Safe Exam Browser", both with version 3.10.0.794.

## Running the modified build

Open `Application/` and double click `SafeExamBrowser.exe`. That starts the modified build from the folder, no installation needed.

- Keep the folder as it is. The exe loads the other assemblies and the Chromium resources from the same directory, so moving a single file out of it will not work.
- Clone with Git LFS installed, otherwise the exe and its DLLs arrive as pointer text and Windows reports them as damaged.
- Supply a `.seb` settings file if the session requires one, and build it with `Application/Configuration/SEBConfigTool.exe`.
- If a session leaves the machine in a blocked state, run `Application/Reset/SafeExamBrowser.ResetUtility.exe`.

## Notes
- Binaries are Git LFS objects. Run `git lfs install` before cloning, otherwise you receive pointer files that cannot execute.
- This environment receives the custom 3.12 package on 15 November 2026. Once it lands, the 3.10.0.794 behaviour recorded here no longer applies to that environment, so the repository is published as a frozen record and will not receive updates.
- Built for education and research. The purpose is to document how the lockdown behaves in a version that is already out of use, and to support an academic defense of that work.
- This project does not condone cheating. Using it to gain an advantage in a live exam, or against any institution's rules, is outside what it is offered for.
- No support. The build is published as is, with no warranty. If it fails to start, breaks a session, or leaves the machine needing a reset, you handle that yourself; there is no channel to report it to and nothing will be fixed.
- Safe Exam Browser is developed by SafeExamBrowser.ch under its own license. This build is a personal research artifact on a machine I control, and is not offered for use in exams at any institution.
