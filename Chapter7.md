## Integrating with Other Technologies

- audio tag is good for background music since built-in buffering and streaming support

- can use media stream audio with Web Audio API through MediaElementAudioSourceNode - behaves like AudioSourceNode but wraps an existing audio tag

```
// feeding an audio element to the Web Audio API
window.addEventListener('load', onLoad, false);

function onLoad() {
	var audio = new Audio();
	source = context.createMediaElementSource(audio);
	var filter = context.createBiquadFilter();
	filter.type = 'lowpass';
	filter.frequency.value = 440;

	source.connect(this.filter);
	filter.connect(context.destination);
	audio.src = 'URL';
	audio.play();
}
```

- detecting whether a page is visible or hidden can be done with Page Visibility API to ensure the integrated sound is that coming from your application and not another tab etc
	- from document.hidden property - a boolean
	- the event which fires is called visibilitychange

```
// Listening to the webkitvisibilitychange event to determine if window is hidden
document.addEventListener('webkitvisibilitychange', onVisibilityChange);

function onVisibilityChange() {
	if (document.webkitHidden) {
		source.stop(0);
	} else {
		source.start(0);
	}
}
```

