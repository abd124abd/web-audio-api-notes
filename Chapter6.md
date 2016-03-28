## Chapter 6 - Advanced topics

- biquad filters - filters can de-emphasize or emphasize certain parts of the frequency spectrum of a sound
	- examples:
		- low-pass filter - more muffled
		- low-shelf filter - influences bass
		- high-shelf filter - influences treble

- BiquadFilterNodes used for applying filters

```
// Create a filter
var filter = context.createBiquadFilter();

filter.type = 'lowpass';
filter.frequency.value = 100;

// then connect the source to it and it to the destination node
```

- getFrequencyResponse() takes an array of frequencies and returns an array of magnitudes of responses relating to each frequency

- ConvolverNode - convolution engine with methods to simulate effects such as chorus, reverberation, telephone speech etc

- spatialized sound - three aspects to complexity here
	- position and orientation of sources and listeners
	- paramaters associated with the source audio cones
	- relative velocities of sources and listeners

- sources can be passed through AudioPannerNode which spatializes input audio and based on the relative positions of sources and listener the API makes gain modifications

