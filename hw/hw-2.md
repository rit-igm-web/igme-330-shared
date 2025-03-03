# HW-2 - Audio Visualizer - *Ultimate Version*

#### Scoring rubric can be found attached to Assignment Dropbox in myCourses.

## Overview
- Build your ***ultimate version*** of the Audio Visualizer!
- Your starting point MUST be the "done" version of [Exercise: Audio Visualizer](../pe/pe-08.md)


---

## I. Refactor the starting code to meet our course coding standards
- [IGME-330 - Course Code Style Requirements](../notes/code-style-required-330.md)
- HTML DOM element `id` values:
  - `playButton` to `btn-play`
  - `volumeSlider` to `slider-volume`
  - `gradientCB` to `cb-gradient` and so on
- Convert ALL regular functions (declared with the `function` keyword) to arrow functions
- ***But don't worry about the sound files names - they don't have to follow the naming standards***


---

## II. User Experience Requirements (25%)
- Add 2 Audio Effect nodes to the audio routing graph: 
  - for example, a "Bass" and "Treble" node - which would be instances of [BiquadFilterNode](https://developer.mozilla.org/en-US/docs/Web/API/BiquadFilterNode)
  - these have been reviewed in **Web Audio WalkThrough** (linked in VII-B. below)
  - the user must be able to control these nodes - for example with either a checkbox or a slider
- Give the user the ability to toggle between the visualization using the ["frequency data"](https://developer.mozilla.org/en-US/docs/Web/API/AnalyserNode/getByteFrequencyData) (what we did already in the AV exercise) and the "time domain" (i.e. waveform) data.
  - The user control could be a checkbox, pull-down (i.e. a `<select>`) or 2 radio buttons
  - The waveform data is the `getByteTimeDomainData` property of the analyzer node, and is documented here: [AnalyserNode.getByteTimeDomainData()](https://developer.mozilla.org/en-US/docs/Web/API/AnalyserNode/getByteTimeDomainData)

---

##  III. Code Requirements (25%)

- Add at least 2 instances of a *Sprite* to the AV
  - Create an ES6 class with `update()` and `draw()` methods (similar to what we did with `CircleSprite`, and in the "Phyllo Classy" checkoff)
  - Periodically animate these sprites and change their state in some way that reflects the change in the audio data
- Make sure that your visualizer does not run at more than 60 FPS on a user's machine:
  - get rid of the `requestAnimationFrame()` code and replace it with [`setTimeout()`](https://developer.mozilla.org/en-US/docs/Web/API/setTimeout)

---

## IV. App Data requirements (10%)

- Create an external JSON file named **av-data.json**
  - Place this JSON file in a folder named **data/**, so that the path to it is **data/av-data.json**
    - do NOT put this JSON file into your **src/** folder
  - Use a JSON validation tool - like https://jsonlint.com/ - to be sure that your JSON is valid and well-formed
- Required data:
  - 1) The **title** of your app
        - this will appear in the `<title>` of the HTML page, and possibly in an `<h1>` on the page
  - 2) The file names of your app's audio files, AND any other associated *meta data* (for example, the name of each track)
  - 3) At least ONE other piece of information - for example:
        - app instructions
        - sprite data (`x`, `y` , `radius`, `outerColor`, `innerColor`, `speed` etc)
        - starting state of UI 
  - Important: structure your JSON data to be a simple as possible, and don't make it any harder to parse than necessary
- Load the data when the app first starts up, and utilize it

---


## V. Aesthetic Requirements (15%)
- Get rid of any vestigal elements of the Audio Visualizer assignment that are less that aesthetically pleasing:
  - for example, that gradient is pretty garish. Either get rid of it entirely or replace it with something more subtle
  - the "pulsing" arcs are not bad, but you could probably modify that code to get something more pleasing, and something that does not look like everyone else's Audio Visualizer
- Try to go "beyond" the canvas drawing we did in class:
  - if you want to keep any of the bitmap effects (ex. "Tint Red"), get rid of the checkboxes and instead give the user more/better controls such as RGB sliders, or "amount of noise" sliders
  - better yet, how about bitmap effects that are *different* from what we saw in the AV assignment? 
- The AV HW just used rectangles and circles (arcs) for drawing  - what else could you use?
  - lines with `ctx.moveTo()` and `ctx.lineTo()`
  - curves with `ctx.arcTo()`, `ctx.bezierCurveTo()`, `ctx.quadraticCurveTo()`
  - gradients and images - animated or otherwise
  - interesting applications of `ctx.translate()`, `ctx.rotate()` and `ctx.scale()`
  - resources - [C2DES - Gradients & Bezier Curves](../notes/7-bezier-curves-and-gradients.md)
- ***To obtain a score in this category, changes MUST be substantially improve the user experience (including aesthetics), appearance and/or functionality of the visualization.***
  - For example, merely changing the colors and the size of the pulsing circles, or modifying the background gradient from one garish combination of colors to another garish combination of colors, will not earn any points in this category
  - For maximum points in this category, think "passion project" or "portfolio piece" that is distinctly different from the Audio Visualizer Exercise and is something you could show to a potential employer
  
---

##  VI. Documentation Requirements (10%)

- Document the following in a plain-text file named **documentation.txt**  -- alternatively, you could create a **documentation.html** file that includes the same information and is linked to from your visualizer, but this is not required.
  - II. Tell us about the 2 Audio Effect nodes you added to the audio routing graph.
  - III. Tell us your Sprite's class name, what it looks like/what it does in the visualization
  - IV. Tell us what app data you put in your **av-data.json** file
  - V. Tell us how you improved the aesthetics of the app over the AV HW:
    - ***be sure to give yourself a grade for this section, between 0% and 15%***
    - 0% would be if there was not a meaningful improvement over the AV HW
    - 15% would be substantial improvement over the AV HW - a "wow" (portfolio-worthy) experience
    - and everything in between

---

## VII. Hints/Tips

### VII-A. Audio Visualizer Examples
- [Audio Visualizer Project Showcase Video (2181)](https://video.rit.edu/Watch/Si56JxGd) - projects are shown starting at 5:00

### VII-B. Resources 
- [Web Audio WalkThrough](../notes/webaudio-walkthrough.md) - covers bass and treble node, as well as better looking drawing with translate/rotate/scale
- [Canvas & Web Audio Resources](../notes/canvas-resources.md)

### VII-C. Creating reusable functions will help a lot

- DRY - multiple parts of your code can call these functions
- you can easily modify function parameters and test your ideas
- your UI can modify these function parameter values
- examples (also see the last version of ScreenSaver for functions you might be able to reuse):

```js
const drawBars = (ctx,audioData,barHeight=200,fillStyle="white", strokeStyle="black",lineWidth=1,numBars) => {
  ...
};

const drawLines = (ctx,audioData,lineWidth=1,strokeStyle="white",magnitude=100,startIndex=0,endIndex) => {
 ...
};
```


### VII-D. Lastly, ***What else can help us create an effective audio visualization?***

1. There should be an analogous relationship between the sound data and what users are seeing on the screen - drawing should not be random like our "screen savers" HW assigments were. Can you instead help your viewer **learn** about sound & music by making new connections, and seeing new patterns, such as:
    - visualizing the "beat"
    - human voices fall into the lower frequencies
    - electronic instruments have a different "shape" than natural instruments

2. Have a good "starting state" to your visualization - the controls should be pre-set to where the visualization has a pleasing state when the user first opens it

3. Don't bore the user - have the visualization periodically change in major ways, automatically. 

4. Give the user controls (sliders, check boxes, pull downs) to effect the visualization. The relationship between what the controls do and what happens on the screen should be obvious

5. Other Tips:
    - Certain drawing could go beyond the raw data for the various bins (frequency ranges), it could instead be aggregate data such as average loudness of all frequencies, or changes in the average of certain frequencies, or tied to beat detection.
    - You can use `ctx.scale()` to squash or stretch shapes - to create ovals for example
    - Not everything has to be drawn at 60 frames/second - use `setTimeout()` or similar to achieve this 
   
6. Get inspired!

    - The power of the canvas is that you can draw *anything* on it!
    - Try googling "audio visualizations" or "data visualizations" and get some ideas!

---

## VIII. Rubric - Attached to Assignment dropbox in myCourses.

### Summary:
- **I. Refactor the code to our course coding standards 10%**
- **II. User Experience Requirements - 25%**
- **III. Code Requirements - 25%** 
- **IV. App Data requirements - 10%**
- **V. Aesthetic Requirements - 15%**
- **VI. Documentation Requirements - 10%**
- ***Starting point is NOT Exercise: Audio Visualizer - (-100%)***
- ***Remaining 5% is following proper submission expectations see below***

---

## IX. Submission

- Put the files from above into a parent folder named ***lastName*-*firstInitial*-hw2**
- ZIP the folder and post to myCourses

---

## X. FAQ
- Can I change the audio files?
  - *Yes, feel free to do so. But keep them SFW - "Safe For Work"*
- Do I have to use a Bass and Treble node, or can I do something else?
  - *The requirements state "Add 2 Audio Effect nodes to the audio routing graph" - the example given was a Bass and Treble effect, but you can use ANY audio effect node that you wish to meet this requirement*
- Regarding the requirement - **Your starting point MUST be the "done" version of Exercise: Audio Visualizer**, does that mean that I have to keep all of the checkboxes, bitmap effects etc the same.
  - *You are free to delete or modify any of the canvas code, checkboxes, digital effects, etc*
  - *What you MUST keep from the Exercise is the custom buttons (an `<audio>` element on the page is not allowed)*
  - *and you MUST retain the JS architecture of the app (i.e. **main.js**, **audio.js**, **utils.js**)*
  - *at a BARE MINIMUM - YOU MUST STILL HAVE THE FOLLOWING CONTROLS from the Exercise (-15% per each one missing or not functional)*:
    - Pause/Play button
    - Volume Slider
    - At least 4 checkboxes that change the visualization in some way (or these could be radio buttons, pull-downs, sliders etc)


