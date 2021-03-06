# PSReleaseTools #

This PowerShell module provides a set of commands for working with the latest releases from the PowerShell [GitHub repository](https://github.com/PowerShell/PowerShell). The module contains commands to get summary information about the most current release as well as commands to download some or all of the release files.

## Cross-Platform
Initial testing has indicated that this module should work cross-platform. If you are running one of the alpha builds of PowerShell v6, even on Linux, you should be able to use this module to download new versions.

## Notes
The module currently has 4 commands:

- [Get-PSReleaseSummary](.\docs\Get-PSReleaseSummary.md)
- [Get-PSReleaseCurrent](.\docs\Get-PSReleaseCurrent.md)
- [Get-PSReleaseAsset](.\docs\Get-PSReleaseAsset.md)
- [Save-PSReleaseAsset](.docs\Save-PSReleaseAsset.md)

> Note: I renamed the save command to use the same noun as `Get-PSReleaseAsset` for consistency after I took the screen shots.

All of the functions take advantage of the [GitHub API](https://developer.github.com/v3/ "learn more about the API") which in combination with either <a title="Read online help for this command" href="http://go.microsoft.com/fwlink/?LinkID=217034" target="_blank">Invoke-RestMethod</a> or <a title="Read online help for this command" href="http://go.microsoft.com/fwlink/?LinkID=217035" target="_blank">Invoke-WebRequest</a>, allow you to programmatically interact with GitHub.

The first command, `Get-PSReleaseSummary` queries the PowerShell repository release page and constructs a text summary.

<a href="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image_thumb.png" alt="image" width="547" height="484" border="0" /></a>

I put the release name and date right at the top so you can quickly check if you need to download something new. In GitHub, each release file is referred to as an <em>asset</em>. The `Get-PSReleaseAsset` command will query GitHub about each file and write a custom object to the pipeline.

<a href="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image-1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image_thumb-1.png" alt="image" width="644" height="298" border="0" /></a>

By default it will display assets for all platforms, but I added a `-Family` parameter if you want to limit yourself to a single entry like MacOS.

<a href="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image-2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image_thumb-2.png" alt="image" width="644" height="192" border="0" /></a>

Of course, you will want to download these files which is the job of the last command. By default the command will save all files to the current directory unless you specify a different path. You can limit the selection to a specific platform via the `-Name` parameter which uses a validation set.

<a href="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image-3.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image_thumb-3.png" alt="image" width="644" height="105" border="0" /></a>

You can select multiple names. If you select only Windows names then there is a dynamic parameter called `-Format` where you can select ZIP or MSI. And the command supports `-WhatIf`.

<a href="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image-4.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image_thumb-4.png" alt="image" width="644" height="54" border="0" /></a>

I also realized you might run `Get-PSReleaseAsset`, perhaps to examine details before downloading. Since you have those objects, why not be able to pipe them to the save command? The command has Filename, Size and URL parameters which accept pipeline input by property name so that you can pipe like this to `Save-PSReleaseAsset`:

<a href="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image-5.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px; border: 0px;" title="image" src="http://jdhitsolutions.com/blog/wp-content/uploads/2017/01/image_thumb-5.png" alt="image" width="644" height="251" border="0" /></a>

The current version of this module uses regular expression named captures to pull out the file name and corresponding SHA256 hashes. The save command then calls `Get-FileHash` to get the current hash and compares them. 

## PowerShell Gallery
This module has been published to the PowerShell Gallery. You should be able to run these commands to find and install it.

    Find-Module PSReleaseTools
    Install-Module PSReleaseTools

If you are on an alpha build of PowerShell v6 and get an error about semantic version, you may need to manually update to the current release and then install this module.
    
## Roadmap
I have a few other ideas for commands I might add to this module. If you have suggestions or encounter problems, please post an issue.

****************************************************************
DO NOT USE IN A PRODUCTION ENVIRONMENT UNTIL YOU HAVE TESTED 
THOROUGHLY IN A LAB ENVIRONMENT. USE AT YOUR OWN RISK. IF YOU DO 
NOT UNDERSTAND WHAT THIS CODE DOES OR HOW IT WORKS, DO NOT USE
OUTSIDE OF A SECURE, TEST SETTING.      
****************************************************************

*Last updated 26 January 2017*
