- allows measurement of the performance of web pages and web apps
```
const performanceEntries = performance.getEntriesByType("resource");
performanceEntries.forEach(entry => {
	console.log(`Name: ${entry.name}, Duration: ${entry.duration}`);
});
```

### ==example== measure performance of sorting algorithms
```
const arrayForBubbleSort = [...largeArray];
const arrayForNativeSort = [...largeArray];

performance.mark("nativeSortStart");
arrayForNativeSort.sort((a, b) => a - b);
performance.mark("nativeSortEnd");

performance.measure("Native Sort Time", "nativeSortStart", "nativeSortEnd");

performance.mark("bubbleSortStart");
bubbleSort(arrayForBubbleSort);
performance.mark("bubbleSortEnd");

performance.measure("Bubble Sort Time", "bubbleSortStart", "bubbleSortEnd");

const nativeDuration =
  performance.getEntriesByName("Native Sort Time")[0].duration;

const bubbleDuration =
  performance.getEntriesByName("Bubble Sort Time")[0].duration;

console.log(`Native Sort Took ${nativeDuration}ms`);
console.log(`Native Sort Took ${bubbleDuration}ms`);
```

### Measuring Resource Load Times
```
performance.getEntriesByType("resource")
```

# Web Audio API
- allows for the processing and synthesizing of audio in web applications.
```
const audioContext = new AudioContext();
const oscillator = audioContext.createOscillator();

oscillator.type = "sine";
oscillator.frequency.setValueAtTime(440, audioContext.currentTime);
oscillator.connect(audioContext.destination);
oscillator.start();
oscillator.stop(audioContext.currentTime + 1);
```

### Control Volume
```
  oscillator = context.createOscillator();

  gainNode = context.createGain();
  gainNode.gain.value = 0.1;

  oscillator.connect(gainNode);
  gainNode.connect(context.destination);
  oscillator.start();
```

> oscillators cannot be started once they are stopped, you will have to create a new oscillator each time

```
let oscillator = null;
let gainNode = null;

playBtn.addEventListener("click", () => {
  oscillator = context.createOscillator();
  oscillator.frequency.value = 440;
  oscillator.connect(gainNode);
  oscillator.start();
});
```

