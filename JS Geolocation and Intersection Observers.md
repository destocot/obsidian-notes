### Geolocation API
- allows users to share their location with web applications
```
if (navigator.geolocation) {
	navigator.geolocation.getCurrentPosition(postion => {
		const { latitude, longittude } = position.coords;
		console.log(`Latittude: ${latitude}, Longitude: ${longitude});
	}
}
```

> `getCurrentPosition` takes two callbacks one for the position objection and one for the error object

> not all browsers support geolocation, `if (navigator.geolocation)`

```
navigator.getlocation.getCurrentPosition(displayGeoData, displayError);
displayGeoData = (position) => { ... }
displayError = (error) => { ... }
```

### `watchPosition`
- we can watch a users position with the `navigator.geolocation` object
```
navigator.geolocation.watchPosition(displayGeoData, displayError);
displayGeoData = (position) => { ... }
displayError = (error) => { ... }
```

---

### MediaStream (`getUserMedia`)
- access the user's camera and microphone
```
async function getMediaStream() {
	try {
		const stream = await navigator.mediaDevices.getUserMedia(
			{ 
				video: true,
				audo: true
			}
		);
		const videoElement = document.querySelector("#videoElement");
		videoElement.srcObject = stream;
	} catch (error) {
		console.error("Error accessing media devices.", error);
	}
}
getMediaStream();
```

### view media devices
```
navigator.mediaDevices.enumerateDevices()
	.then((devices) => {
		console.log(devices);
	})
	.catch((e) => console.log(e));
```

---

### Intersection Observer API
- provides a way to asynchronously observe changes in the intersection of a target element with its parent or the viewport.
```
const observer = new IntersectionObserver(
  (entries) => {
    entries.forEach((entry) => {
      const { isIntersecting } = entry;
      const { id } = entry.target;
      if (isIntersecting) {
        console.log(`${id} ad is visible`);
      } else {
        console.log(`${id} ad is NOT visible`);
      }
    });
  }
);

const ads = document.querySelectorAll(".ad");
ads.forEach((ad) => observer.observe(ad));
```

### Threshold options
> allows to control how much of the entry should we see before we are "intersecting"

```
const observer = new IntersectionObserver(entries => {
	entries.forEach(entry => { 
		if (entry.isIntersecting) {
			const percentage = Math.round(entry.intersectionRatio * 100);
			console.log(`${percentage}%`);
		}
	})
}, { threshold: [0, 0.25, 0.5, 0.75, 0.1 ]})
```

### ==example== track how long an AD is visible
```
let adViewTimes = [];
let adVisibleStartTime;

const observer = new IntersectionObserver(
	(entries) => {
		entries.forEach((entry) => {
			const { isIntersecting } = entry;
			if (isIntersecting) {
				//ad is visible
				adVisibleStartTime = Date.now();
			} else if (adVisibleStartTime) {
				//ad has been visible, no longer is visible
				let adViewDuration = Date.now() - adVisibleStartTime;
				adViewTimes.push(adViewDuration);
				console.log(`Ad was viewed for ${adViewDuration} ms`);
				adVisibleStartTime = undefined;
			}
		});
	},
	{ threshold: 0.5 }
);

```

### ==example== lazy loading

```
<img class="lazy" alt="An image" data-src="<actual-image-src"> />
```

```
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      //Image needs to be loaded!!!
      console.log("LOADING A NEW RANDOM IMAGE!!!");
      
	  const datasetSrc = entry.target.dataset.src;
      entry.target.src = datasetSrc ?? "https://source.unsplash.com/random";
      observer.unobserve(entry.target);
    }
  });
});

const allImages = document.querySelectorAll("img.lazy");
allImages.forEach((img) => observer.observe(img));
```

