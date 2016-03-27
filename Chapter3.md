## Chapter 3 - Volume and Loudness

- GainNodes have a .gain parameter acting as a multipler
	- 0-1 reduce the loudness
	- 1+ increase
	- and neg values invert the waveform/amplitude

- loudness - subjective measure of how intensely a sound is perceived by our ears
- volume - measure of the physical amplitude of a sound wave
- gain - scale multiplier affecting a sounds amplitude as its being processed

- power in a wave is measured in decibels(dB) or 1/10 a Bel(Alexander Graham Bell)

- dB are a logarithmic measure relative to a reference point, dB alone is meaningless

- digital sound relevance, dBFS and dbSPL

- dBFS - decibels full scale - highest possible is 0 
	- measure of gain not volume

- dbSPL - decibels relative to sound pressure level - most common everyday useage
	- reference point is .000002 newtons per square meter
	- ear damage can occur at ~120 dbSPL
	- Web Audio API only uses dBFS since the final volume is predicated upon the OS and speaker gain levels

- Equal power crossfading
	- gradual cross-fading between sounds for smoother transitions (ie game scenes with diff music)
	- equal power curve is used
	- corresponding gain curves are neither linear nor exponential
	- they intersect at a higher amplitude
	- this helps prevent a dip in volume in the middle part of the crossfade as would be experienced with a linear fade

```
// generate an equal power crossfade
function equalPowerCrossfade(percent) {
	var gain1 = Math.cos(percent * 0.5 * Math.PI);
	var gain2 = Math.cos((1.0 - percent) * 0.5 * Math.PI);
	this.ctl1.gainNode.gain.value = gain1;
	this.ctl2.gainNode.gain.vlaue = gain2;
}
```

- clipping removes sound if waveform exceeds its maximum level - green, yellow, red
	- Web Audio API uses 16-bit audio, the floating point version of the signal the bit values are mapped to [-1, 1]
	- leave headroom to prevent clipping - ensure sound level is below the max(1) esp when playing sounds together

```
// check for and save clipping time
function onProcess(e) {
  var leftBuffer = e.inputBuffer.getChannelData(0);
  var rightBuffer = e.inputBuffer.getChannelData(1);
  checkClipping(leftBuffer);
  checkClipping(rightBuffer);
}

function checkClipping(buffer) {
  var isClipping = false;
  // Iterate through buffer to check if any of the |values| exceeds 1.
  for (var i = 0; i < buffer.length; i++) {
    var absValue = Math.abs(buffer[i]);
    if (absValue >= 1.0) {
      isClipping = true;
      break;
    }
  }
  this.isClipping = isClipping;
  if (isClipping) {
    lastClipTime = new Date();
  }
}
```

	- other options against clipping are fractional master audio gain node or dynamics compression

- dynamic range - difference between loudest and quietest parts of a sound
	- dynamics compression lowers the variance of the range while making the sound louder overall

- compressor nodes can help in suddenly loud situations
	- main parameters are threshold and ratio
	- threshold lowest volume compressor starts reducing the dynamic range
	- ratio is how much gain reduction is applied by the compressor

- compressors available as DynamicsCompressorNodes()

```
// dynamics compressor nodes usually best as last node in your audio graph before your destination node
var compressor = context.createDynamicsCompressor();
mix.connect(compressor);
compressor.connect(context.destination);
```
