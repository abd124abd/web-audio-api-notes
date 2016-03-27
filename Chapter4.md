## Chapter 4 - Pitch and Frequency Domain

- sounds are periodic waveforms (tones)
- pitch is the periodicity of the wave
- pitch of a note is measured in frequency(hz) (number of times the wave repeats per second)
- the frequency is the time in seconds between every wave crest

- cutting the time of the wave in half results in double the frequency (same tone but one octave higher in pitch)
- extending the wave's frequency by 2 extends the time by 2 which lowers the pitch by half (same tone but once octave lower)

- pitch like volume is percieved exponentially by our ears -> 1 octave = 2x frequency

- an octave is comprised of 12 semitones
- Web Audio API includes a detune that linearizes the frequency to a measure of cents
	- each octave consists of 1200 cents and one semitone is 100 cents
	- thus +1 octave = +1200 cents, -1 octave = -1200 cents

- Web Audio API pitch influenced by playbackRate() parameter
- multiply the desired frequency by the semitone ratio 2^1/12 to set pitch

```
// raw semitone method
function playNote(semitones) {
	var semitoneRatio = Math.pow(2, 1/12);
	source.playbackRate.value = Math.pow(semitoneRatio, semitones);
	source.start(0);
}

// Web Audio API cents detuning re-write
function playNote(semitones) {
	source.detune.value = semitones * 100;
	source.start(0);
}
```

- frequency domain - frequency on x-axis and amplitude on y-axis for the frequency analysis of a waveform at one point in time
	- more informative than time down grpah
	- utilize things like Fast Fourier Transform(FFT) to get a sine wave decomposition for a frequency domain plot

- Web Audio API comes with OscillatorNode which has configurable frequency and detune
	- also have built in types such as sine, triangle, sawtooth and square waves

```
// example of Oscillator audio graph in place of AudioBufferSourceNode
function play(semitone) {
	// create the node
	var oscillator = context.createOscillator();
	oscillator.connect(context.destination);

	// play a sine type curve at A4 frequency (440hz)
	oscillator.frequency.value = 440;
	oscillator.detune.value = semitone * 100;

	oscillator.type = oscillator.SINE;
	oscillator.start(0);
}
```


