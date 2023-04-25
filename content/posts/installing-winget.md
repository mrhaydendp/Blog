---
title: "Installing Winget"
author: "Hayden Plumley"
date: "2022-05-16"
description: "How to install Winget in a few simple commands."
tags: ["Windows"]
categories: ["Tutorials"]
---

*Updated: January 4, 2023*

Winget is a package manager based off [Keivan Beigi's AppGet](https://keivan.io/the-day-appget-died/), its goal is to be a "one-stop shop" to install all your Windows programs. It achieves this by having a central repository of installation scripts for all of your favorite applications such as Discord, Spotify, and Steam. ~~Currently, Winget has to be manually installed, but later it should come bundled with Windows.~~

## Windows Store Method
Direct Link to Winget (AppInstaller) in Windows Store:
- [View in Store](ms-windows-store://pdp/?ProductId=9NBLGGH4NNS1)

## PowerShell Method
Here's a one-liner to get Winget installed through Powershell:
``` powershell
# Download Winget installer from Microsoft & launch it
Start-BitsTransfer "https://aka.ms/getwinget" getwinget.msixbundle; .\getwinget.msixbundle
```

## Winget Basics
| Command                    | Description                     |
|----------------------------|---------------------------------|
| `winget install "appname"`   | Install specified application   |
| `winget install -s source "appname"` | Select Winget's installation source (msstore, winget) |
| `winget uninstall "appname"` | Uninstall specified application |
| `winget upgrade "appname"`   | Update specified application    |
| `winget upgrade --all`       | Update all supported applications   |

