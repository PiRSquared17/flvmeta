# About FLVmeta #

FLVmeta aims to be a replacement for tools such as flvtool2.

It has the ability to inject all standard metadata tags into the onMetaData event, as well as insert an onLastSecond event.

Since version 1.0.7, it is also able to fix very large FLV files with invalid 24-bit timestamps to make proper use of 32-bit extended timestamps.

FLVmeta is written in portable C, and can therefore be compiled on a variety of platforms, including Linux, Windows, and MacOSX.
It is also pretty fast and has a very small memory footprint, making it ideal for use as an automated tool on server environments.

# News #

## 2013/05/26 ##

The project has been moved quite some time ago to Github (https://github.com/noirotm/flvmeta). As a result, development is **not** happening anymore on the Google Code project page.
To make project maintenance more consistent, google code is progressively being phased out.

To begin with, open issues have been moved to github, as well as Wiki pages.
The Git repository will not be updated anymore, since it was just a Github mirror anyways, and has never been up-to-date anyways.

Downloads are kept for historical reasons, but as Google announced in a blog post, Google code spport for file downloads has officially ended, and they **will** disappear in january 2014. Binary distributions and official archives can be found at http://www.flvmeta.com/.

## 2012/05/03 ##

FLVmeta 1.1.0 has been officially released. You can download a package for your favorite platform at http://code.google.com/p/flvmeta/downloads/list.

It is considered a beta release, but has been tested in a production environment for a long time, and therefore should be pretty stable.

## 2012/04/22 ##

Due to the migration of the repository to Git, the snapshot releases have a new naming convention. Instead of reflecting an increasing integer number, they are now tagged with the seven first characters from the corresponding git commit's SHA hash.

The most recent release (g7c935d3) adds support for the --check command, and fixes a few bugs.

## 2011 ##

FLVmeta 1.1 is nearing completion.

Snapshots can be downloaded at: http://code.google.com/p/flvmeta/downloads/list

A lot of new features have been added, here is an exhaustive list:
  * Added proper command line handling and help.
  * Added the possibility to overwrite the input file when the output file is not specified or when both files are physically the same.
  * Added support for CMake builds in addition to autotools. It is now the official way to build flvmeta on Windows.
  * Added metadata and full file dumping, integrating former flvdump functionality into flvmeta.
  * Added support for XML, YAML, and JSON formats for dumping.
  * Added XML schemas describing the various formats used by flvmeta.
  * Added a file checking feature
  * Added the possibility to print output file metadata after a successful update using one of the supported formats.
  * Added a feature to insert custom metadata strings while updating.
  * Added an option to disable insertion of the onLastSecond event.
  * Added an option to preserve existing metadata tags if possible.
  * Added an option to fix invalid tags while updating (this is a highly experimental feature, should be used with caution)
  * Added an option to ignore invalid tags as much as possible instead of exiting at the first error detected.
  * Added an option to reset the file timestamps in order to correctly start at zero, for example if the file has been incorrectly split by buggy tools
  * Added an option to display informative messages while processing (not quite exhaustive for the moment)

&lt;wiki:gadget url="http://www.ohloh.net/projects/19570/widgets/project\_partner\_badge.xml" height="53"  border="0" /&gt;

[![](http://mac.softpedia.com/base_img/softpedia_free_award_f.gif)](http://mac.softpedia.com/progClean/FLVMeta-Clean-40458.html)