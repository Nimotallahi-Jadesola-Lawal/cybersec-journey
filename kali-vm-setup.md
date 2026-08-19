# Setting Up Kali Linux in VirtualBox: My First Home Lab Build

**Date:** August 4, 2026
**Goal:** Get Kali Linux running inside VirtualBox on my Dell Latitude 5420 (Windows 11, 16GB RAM)

This took way longer than I expected, several hours and about five different errors. Documenting the whole thing here, mistakes included, because I know future-me (and probably someone else starting out) will need this.

## What I installed

1. **VirtualBox 7.2.14**  downloaded from virtualbox.org, installed with default settings
1. **Kali Linux 2026.2 (VirtualBox pre-built image)**  downloaded from kali.org/get-kali, under “Virtual Machines” (not “Installer Images”  that’s for installing directly on hardware, which I didn’t want)

## The problems I hit (and how I fixed them)

### Problem 1: VirtualBox pointed to a temp folder that didn’t exist

First import attempt failed with “Could not open the medium.” Turned out VirtualBox’s `.7z` extraction process had registered the disk file using a path inside `AppData\Local\Temp\...`  a temporary folder that Windows had already cleared out.

**Fix:** Removed the broken VM entry entirely, made sure the Kali files were fully extracted into a permanent folder (Downloads), and re-pointed VirtualBox there.

### Problem 2: OneDrive sync interference

At one point I’d moved the extracted Kali folder onto my Desktop, which on this machine is synced through OneDrive. VirtualBox couldn’t reliably open the disk file when it lived inside a OneDrive-synced folder.

**Lesson learned:** Keep large VM files in a folder that is NOT synced to OneDrive (like Downloads, or a dedicated local-only folder). OneDrive sync and virtual disk files don’t play well together.

### Problem 3: Windows silently blocking the downloaded file

Checked file Properties and found a note: *“This file came from another computer and might be blocked to help protect this computer.”* This is Windows’ “Mark of the Web” security feature for downloaded files.

**Fix:** Right-click the file → Properties → check the **Unblock** checkbox → Apply.

### Problem 4: VirtualBox UUID conflict

Even after fixing the path and unblocking the file, VirtualBox kept refusing to attach the disk with the error “a hard disk with UUID {…} already exists.” This happened because the broken temp-folder version and my new Downloads copy shared the same internal UUID (since one was originally copied from the other).  VirtualBox treats this as a duplicate and blocks it.

**Fix:** Fully removed the VM **and its files** from VirtualBox (right-click → Remove → “Delete the virtual machine files and virtual hard disks”). This cleared VirtualBox’s internal registry of the broken UUID.

### Problem 5: Import only accepts .ova/.ovf my file was .vbox

After the full removal, I tried “Import” again, which only shows `.ova`/`.ovf` files. My Kali download only included `.vbox` + `.vdi` files, no `.ova`. Import was never going to work for this specific download.

**Fix:** Used **File → Open** instead of Import. Open accepts `.vbox` files directly (their native format), which is exactly what I had. This is what finally worked.

## What actually worked, step by step

1. Fully deleted the broken VM (Remove → Delete files)
1. In VirtualBox: **File → Open**
1. Navigated to `Downloads > kali-linux-2026.2-virtualbox-amd64`
1. Selected the `.vbox` file (VirtualBox Machine Definition, small file, blue cube icon)
1. Clicked Open  it loaded straight into the VM list, no import/conversion needed
1. Clicked **Start**
1. Logged in with username `kali`, password `kali`

## Key takeaways

- **VM files should live in a plain local folder  not OneDrive, not Desktop if Desktop is synced.**
- **Check file Properties for “blocked” downloads before troubleshooting anything else**; this can look like a totally unrelated error.
- **`.vbox` files use “Open,” not “Import.”** Import is specifically for `.ova`/`.ovf` packages.
- When VirtualBox complains about a UUID already existing, the fix is a full Remove + re-add, not just retrying the same attachment.
- Debugging this took patience over cleverness; each error message, read carefully, pointed at the actual next step.

## Next steps

- [ ] Explore the Kali desktop, get familiar with the terminal (in progress)
- [ ] Complete TryHackMe Pre Security path, alongside Security+ study
- [ ] Move to TryHackMe SOC Level 1 path after Pre Security is complete
- [ ] Set up Metasploitable2 as a first vulnerable target (moved later in sequence, after foundational TryHackMe rooms are complete)
- [ ] Continue documenting progress here and on LinkedIn weekly
