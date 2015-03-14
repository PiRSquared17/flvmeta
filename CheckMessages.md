# FLVMeta check command message codes and conventions #

## Conventions ##

Messages are divided into four specific levels of increasing importance :

  * **info**: informational messages that do not pertain to the file validity
  * **warning**: messages that inform of oddities to the flv format but that might not hamper file reading or playability, this is the default level
  * **error**: messages that inform of errors that might render the file impossible to play or stream correctly
  * **fatal**: messages that inform of errors that make further file reading impossible therefore ending parsing completely

Each message or message template presented to the user is identified by a specific code of the following format:

```
 [level][topic][id]
```

  * **level** is an upper-case letter that can be either I, W, E, F according to the previously defined message levels.
  * **topic** is a two-digit integer representing the general topic of the message.
  * **id** is a unique three-digit identifier for the message, or message template, in the topic.

Example:
> W10050 : represents a Warning in topic 10 with the id 050.

The actual message can vary because it can be localized in various languages.
Automated analysis should be performed by using the information provided in this document.

## Topic list ##

Messages can be related to the following topics :

  * **10** general flv file format
    * **11** file header
    * **12** previous tag size
  * **20** tag format
  * **30** tag types
  * **40** timestamps
  * **50** audio data
    * **51** audio codecs
  * **60** video data
    * **61** video codecs
  * **70** metadata
  * **80** AMF data
    * **81** keyframes
    * **82** cue points

## Checks to perform ##

### Globally ###
  * F if magic number != FLV
  * F if EOF in header
  * E if we have no video and no audio in the header
  * W if we don't have video
  * W if audio/video flag is not consistent with the file contents (presence of audio/video streams in the tags)
  * E if version is not 1
  * E if the header's reserved flags != 0, or offset is not 9
  * E if the 1st previous tag size != 0
  * I informations about codecs and streams
  * I information about the minimum flash player version supported for this file
  * I if the file is larger than 4 GB
  * I if the file uses extended timestamps
  * F if there is no tag after the header

### for each tag ###
  * F if EOF in tag
  * E if the tag type is not video/audio/data
  * W if body length is abnormally large (todo: try to determine the maximum acceptable size for tag bodies)
  * E if first timestamp is not zero (usually the onMetaData)
  * E if timestamp decreases for the same tag types
  * E if timestamp's extended bits are not used after overflow
  * W if there's a huge desync between audio and video timestamps
  * W if one stream stops earlier than the other
  * W if streamID != 0
  * F if we reach EOF before reading body length bytes
  * F if EOF in previous tag size
  * E if previous tag size is not 11 + body length

### for audio tags ###
  * E if sound format is unknown or reserved
  * W if sound format/rate/size/type varies between tags
  * W if type == stereo and format == nellymoser
  * W if type == mono and format == aac

### for video tags ###
  * E if frame type is unknown
  * E if codec is unknown
  * W if codec varies between tags
  * W if first video frame is not a keyframe (playback may suffer)
  * W if no keyframe at all (incomplete file ?)
  * W if only keyframes (inefficiency)
  * E if only keyframes AND presence of onLastSecond (known flash player bug)
  * W if unable to determine video width and height from video data

### for data tags ###
  * W if onLastSecond tag is relatively far from the last second (if present)
  * W if tag name is empty
  * W if there are more than one onMetaData or onLastSecond
  * E if AMF data are not readable
  * W if AMF data size != body length
  * I if we meet an unknown event name (we need to check what common values are)
  * W if no onMetaData tag in the file
  * W if onMetaData is not the first tag or doesn't have a 0 timestamp

### onMetaData ###
  * W for incorrect values (we need to perform a thorough check beforehand, most likely to be tested after having read the whole file)
  * W for absence of keyframes/filepositions
  * W for empty keys in associative arrays/objects
  * W for invalid UTF-8 string values