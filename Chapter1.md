## Chapter 1 - Fundamentals

- <audio> tag = native support for all major browsers, but limitations:
	- no precise timing controls
	- low limit of sounds at once
	- no reliable pre-buffering
	- no real-time effects
	- no way to analyze sounds

- Web Audio API audo context
	- a set of nodes which define how an audio stream flows from its source to its destination
	- as audio passes through a node, its properties can be influenced
	- simplest example is a source node leading directly to a destination node
	- there can be many incoming/outgoing connections to a node
	- multiple incoming streams are blended by the API

- One of the main uses of audio contexts is to create audio nodes

- types of audio nodes:
	- source nodes - sound sources like audio buffers, live audio inputs, <audio> tags, oscillators, and JS processors
	- modification nodes - filters, convolvers, panners, JS processors
	- analysis nodes - analyzers and JS processors
	- destination nodes - audio outputs and offline processing buffers
	
- input and output node's connected using connect() function, example

```
// Create the source.
var source = context.createBufferSource();
// Create the gain node.
var gain = context.createGain();
// Connect source to filter, filter to destination.
source.connect(gain);
gain.connect(context.destination);
```

- connections can be dynamically changed as well, for example disconnect()

```
source.disconnect(0);
gain.disconnect(0);
source.connect(context.destination);
```

- what is sound?
	- longitudinal / pressure wave travelling through air between mediums
	- sound source causes molecules in the air to vibrate and collide
	- this creates high and low pressure molecular regions in the air, which come together and fall apart in bands
	- mathematically sound is a function with pressure value ranges 
		- high values = dense particles bands / high pressure
		- low values = sparse particles / low pressure

- what is digital sound?
	- created by collecting numerical representations of analog sound at various frequencies
	- these representations are called samples, and they are numbers within a range called the bit-depth (16-bit, 24-bit, etc)
	- a 44.1khz sample rate means there are 44,100 numbers recorded each second

- converting analog signals to digital is called quantization (or sampling)

- pulse-code modulation (PCM) - encoding sound such that computers can treat digitized sound as a long array of numbers

- Web audio API PCM abstracted as AudioBuffer - which can store multiple audio channels and represented as floating point numbers between -1, 1 or integers within their bit-depth range

- 2 kinds of compression
	- lossless - remains identical
	- lossy - intelligently removes bits that we likely won't be able to hear anyways to save memory (.mp3s do this)

- bit-rate, not to confused with sample rate(eg 44.1khz) and bit-depth(eg 16-bit) refers to the number of bits of audio required per playback (eg 128 kb/s)
	- higher the bit-rate (bits of audio at playback) the less compression required

- buffer and source-node distinction
	- idea is to decouple the audio asset from the playback state
	- buffer is the 'record' and source-node is the 'playhead'
	- the buffer can be played with any number of 'playheads' simultaneously
	- ex. for multiple ball bouncing sounds you load the bounce buffer once, and schedule multiple sources of playback(source-nodes)

- can load sound through XMLHttpRequest() and process asynchronously with context.decodeAudioData()

- creating source node and playing sound after loading buffer
	
```
function playSound(buffer) {
// create the source node
  var source = context.createBufferSource();
// load buffer into source node
  source.buffer = buffer;
// connect source node buffer to destination node
  source.connect(context.destination);
// play
  source.start(0);
}
	```

- ex JavaScript abstraction class for putting everything together into a loader and returns audio buffers

```
window.onload = init;
var context;
var bufferLoader;

function init() {
  context = new webkitAudioContext();

  bufferLoader = new BufferLoader(
    context,
    [
      '../sounds/hyper-reality/br-jam-loop.wav',
      '../sounds/hyper-reality/laughter.wav',
    ],
    finishedLoading
    );

  bufferLoader.load();
}

function finishedLoading(bufferList) {
  // Create two sources and play them both together.
  var source1 = context.createBufferSource();
  var source2 = context.createBufferSource();
  source1.buffer = bufferList[0];
  source2.buffer = bufferList[1];

  source1.connect(context.destination);
  source2.connect(context.destination);
  source1.start(0);
  source2.start(0);
}
```

