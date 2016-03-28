## Chapter 5 - Analysis and Visualization

- AnalyserNode - to analyze a sound, can be placed anywhere, do not change the sound in anyway, frequency domain and time domain analysis
	fftSize - buffer size used to perform analysis, values must be powers of 2, higher values = more accurate but less performant
	frequencyBinCount - read-only property automatically set to fftSize/2
	smoothingTimeConstant - value between 0,1, 1 = large moving avg window and smoothed results

```
// basic analyzer setup
var analyser = context.createAnalyser();
A.connect(analyser);
analyser.connect(B);

// get frequency domain as an example, could be time domain instead
var freqDomain = new Float32Array(analyser.frequencyBinCount);
analyser.getFloatFrequencyData(freqDomain);
```

- freqDomain here is an array of 32-bit floats corresponding to the frequency domain
- these values are between 0,1
- the indexes of the output can be mapped linearly between 0 and the nyquist frequency - half the sampling rate (ex. .5*44,100)
- the nyquist frequency is found at context.sampleRate/2

- visualizing sound examples - through a render loop that queries and renders the analyzer frequency analysis

```
// freqDomain example
// Uint8Array (256-bit values) should be the same length as the frequencyBinCount (buffer size/length of the audio)
var freqDomain = new Uint8Array(analyser.frequencyBinCount); 
// fill the Uint8Array with data returned from the getByteFrequencyData() method
analyser.getByteFrequencyData(freqDomain);
for (var i = 0; i < analyser.frequencyBinCount, i++) {
	var value = freqDomain[i];
	var percent = value / 256;
	var height: HEIGHT * percent;
	var offset = HEIGHT - height - 1;
	var barWidth = WIDTH/analyser.frequencyBinCount;
	var hue = i/analyser.frequencyBinCount * 360;
	drawContext.fillStyle = 'hsl(' + hue + ', 100%, 50%)';
	drawContext.fillRect(i * barWidth, offset, barWidth, height);
}

// timeDomain example
var timeDomain = new Uint8Array(analyser.frequencyBinCount);
analyser.getByteTimeDomainData(timeDomain);
for (var i = 0; i < frequencyBinCount; i++) {
	var value = timeDomain[i];
	var percent = value / 256;
	var height = HEIGHT * percent;
	var offset = HEIGHT  - height - 1;
	var barWidth = WIDTH/analyser.frequencyBinCount;
	drawContext.fillStyle = 'black';
	drawContext.fillRect(i*barWidth, offset, 1, 1);
}
```

