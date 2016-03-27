## Chatper 2 - Perfect Timing and Latency

- Web Audio API comes with a low-latency precise timing model - ie feedback is immediate (low latency) and precise timing 
allows for scheduling of events at specific times in the future

- currentTime property in this audio context is in seconds, floating point, and very accurate

- make sure to load buffers before attempting to load (start()) the sound

- to start a sound with delay use currentTime +

```
start(context.currentTime + 5) // to start sound after 5 seconds
```

- once a source node has finished playing it cant play back again, need to create new source node and call start() again

- once a sound is scheduled for the future it cannot be unscheduled

- can change audio paramaters such as gain through AudioParam instances ie gainNode.gain.value = 0.5 to reduce the volume

- for gradual transitions to parameters can use linearRampToValueAtTime() and exponential...()

