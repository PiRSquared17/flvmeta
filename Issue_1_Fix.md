# Introduction #

Ref: http://code.google.com/p/flvmeta/issues/detail?id=1

flvmeta (1.0 and 1.1) does not correctly handle the case where
onMetadata is not the first tag, or when there are several data tags before audio and video tags.

# Details #

Even though their size is taken in account, every data tag which name is **not**
`onMetaData` is skipped when being read in the first pass.

When we start to write the output FLV file, we first write the header, and then the computed `onMetaData` tag, and then the following tags in the file.

The main problem here is that we actually wait for the first **non-data** (understand video or audio) tag as a start point to start copying tags verbatim, discarding all data tags that occured before, because it was believed that only the `onMetaData` tag would be encountered before the audio and video tags.

Therefore, the offsets will have been computer correctly, but every data tag before the first non-data tag will be missing in the output file, with the exception of the `onMetaData` tag which has been computed and written by flvmeta, superseding the one eventually found in the input file.

# Solution #

A few cases can happen.

  1. The input file contains no data tags before the first audio/video tag.
  1. The input file contains an `onMetaData` tag before audio/video tags.
  1. The input file contains one or more data tags before audio/video tags but no `onMetaData` tag.
  1. The input file contains one or more data tags and the `onMetaData` tag before the first audio/video tag.

In the current implementation, only the case 1 and 2 are handled. We can generalize a solution from the other cases.

If there is no `onMetaData` tag found in the input file, we will write the computed one as the first tag in the output file.

If there is an `onMetaData` tag found in the input file, we will start the verbatim copies from the first tag in the input file, and only replace the first `onMetaData` found.

# Update #

This issue has been fixed in the trunk.

Backporting the fix to the 1.0.x branch will require some efforts, since the reading/writing code has been substancially altered since then.