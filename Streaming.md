# Introduction #

The idea is to have flvmeta read an flv file and stream it at the speed computed from the file's timestamps, achieving real-time streaming, in order to reduce bandwidth usage introduced by usual flv streaming modules.

We should be able to output the data to stdout (by default), or any file/device, or a TCP/UDP socket.

We should also have the possibility to burst streaming speed for N seconds, in order to allow the client to bufferize enough data to begin playback immediatly, and then resuming streaming at the nominal file bitrate.

# FLVmeta command line alteration #

```
  -S, --stream                   stream INPUT_FILE to device or socket. If OUT_FILE
                                 is omitted, defaults to stdout.

Stream options:
  -h, --host=HOST:PORT           stream file to host/port ignoring OUT_FILE
  -u, --udp                      stream file to udp socket
  -t, --tcp                      stream file to tcp socket
  -b, --burst=DURATION           burst streaming for DURATION milliseconds
  -o, --offset=BYTES             start streaming file at BYTES offset 
```

# Details #

## Algorithm ##

The simplest way to do streaming that way is to get the system timestamp at start of playback, then for each tag, read it, and wait for the difference between this tag and the previous, then send it verbatim.

We need to be precise enough, basically we should be able to wait for 1 msec. nanosleep (http://linux.die.net/man/2/nanosleep) seems the best function on Unix.

Sleep is the best alternative on Windows.

## Random access seeking ##

If we have to start at a given offset, we need to send a fake header first, or simply send the real header itself (which seems more correct anyways), then seek into the file and start streaming from the given offset.

It must be noted that to be able to stream from a random byte offset, we must still be exactly at the start of a tag, in order to keep track of timestamps.

One solution is to check the file's keyframes list in the metadata so we can verify whether we're on a keyframe, and eventually seek to the next keyframe.
This implies the file must contain keyframe informations.
We should issue an error in all possible cases.