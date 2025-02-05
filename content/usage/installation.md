+++
title = "Installation"
description = "Cockpit installation instructions."
date = 2024-12-30T04:30:00+11:00
template = "docs/page.html"
sort_by = "weight"
weight = 10
draft = false

[extra]
toc = true
top = false
+++

## BlueOS Extension

For vehicles with an [Onboard Computer](https://blueos.cloud/docs/stable/integrations/hardware/required/onboard-computer/) running [BlueOS](https://blueos.cloud/docs/),
and an IP-based (wifi / ethernet tether) connection to the [Control Station Computer](https://blueos.cloud/docs/stable/integrations/hardware/required/control-computer/),
Cockpit [is available](https://docs.bluerobotics.com/BlueOS-Extensions-Repository/#:~:text=Cockpit,-Maintainer)
as a [BlueOS Extension](https://blueos.cloud/docs/stable/development/extensions/).

### Extension Updates

Once installed, updating to a new Cockpit version can be done via the "Installed" tab of the BlueOS
[Extensions Manager](https://blueos.cloud/docs/stable/usage/advanced/#extensions-manager).


## Self-Contained Application

Cockpit is also available as a self-contained Electron application, which can be stored on a Control Station
Computer and started up for connection to a vehicle.

The latest available release is

[![Latest Stable](https://img.shields.io/github/v/release/bluerobotics/cockpit.svg?label=Cockpit) ![Date](https://img.shields.io/github/release-date/bluerobotics/cockpit?label=Date)](https://github.com/bluerobotics/cockpit/releases/latest)

Download the latest version for your operating system here:

{% horizontal_scroll(width="750px") %}
| Operating System | x86_64 | arm64 |
| --- | --- | --- |
| Windows | <a id="win-x64">Cockpit.exe</a> | Not available |
| macOS[¹](#1) | <a id="mac-x86_64">Cockpit-Intel.dmg</a> | Not yet available, use x86_64 version |
| iOS / iPadOS | N/A | use the BlueOS Extension in a browser |
| Linux | <a id="linux-x86_64-AppImage">Cockpit-x86_64.AppImage</a><br><a id="linux-x86_64-flatpak">Cockpit-x86_64.flatpak</a> | <a id="linux-arm64-AppImage">Cockpit-arm64.AppImage</a><br><a id="linux-arm64-flatpak">Cockpit-arm64.flatpak</a> |
| Android | N/A | use the BlueOS Extension in a browser |
{% end %}

or check the [releases](https://github.com/bluerobotics/cockpit/releases), for a list of all available versions, and the main changes between them.

[^1]: Cockpit is not yet registered with Apple, so may get flagged as a potential security threat. For now, the first open on macOS may require
      right-clicking Cockpit in your Applications folder, selecting "Open", then choosing to "Open anyway" if prompted, or opening the security
      preferences and scrolling down to "Allow" opening if there is no prompt.


### Application Updates

The Cockpit application checks for new versions each time it opens, and if the latest available version
is newer than the installed one, it provides some information about the new release, and a button for if
you want to download and install it.

<script type="text/javascript">
async function fetchLatestReleaseInfo() {
  const url = "https://api.github.com/repos/bluerobotics/cockpit/releases/latest";
  
  const response = await fetch(url)
  if (!response.ok) {
    throw new Error(`Failed to fetch latest release info: ${reponse.statusText}`)
  }

  const info = await response.json()
  return info
}

function setLinkURL(aID, artifact) {
  try {
    document.getElementById(aID).setAttribute("href", artifact.browser_download_url);
    // console.log(`Set ${aID} link to ${artifact.name} file download URL.`)
  } catch (error) {
    console.error(`Failed to set ${aID} link: ${error.message}`)
  }
}

async function setDownloadURLs() {
  try {
    const releaseInfo = await fetchLatestReleaseInfo()
    releaseInfo["assets"].forEach((artifact) => {
      const name = artifact.name;
      switch (name.substring(name.lastIndexOf("."))) {
        case ".exe":      // Windows
          setLinkURL("win-x64", artifact);
          break;
        case ".dmg":      // macOS
          setLinkURL("mac-x86_64", artifact);
          break;
        case ".AppImage": // Linux (AppImage)
          if (name.includes("x86_64")) {
            setLinkURL("linux-x86_64-AppImage", artifact);
          } else if (name.includes("arm64")) {
            setLinkURL("linux-arm64-AppImage", artifact);
          }
          break;
        case ".flatpak":  // Linux (flatpak)
          if (name.includes("x86_64")) {
            setLinkURL("linux-x86_64-flatpak", artifact);
          } else if (name.includes("arm64")) {
            setLinkURL("linux-arm64-flatpak", artifact);
          }
          break;
        default:
          // some other (unused) artifact
      }
    })
  } catch (error) {
    console.error(`Error: ${error.message}`)
  }
}

setDownloadURLs()
</script>
