<script setup lang="ts">
// Define the data structure for our circle of fifths
import {computed, onMounted, ref} from "vue";

const notest = {
  sound: ['G#2', 'G#2', 'G#2','G#2', 'G#2','G#2', 'A#4', 'C#5','D#4', 'F#2','G#2', 'A#4'],
  displayData: ['about C', 'about G ', 'about D', 'about A', 'about E', 'about B', 'about F#', 'about Db', 'about Ab', 'about Eb', 'about Bb', 'about F'],

  major: ['C', 'G', 'D', 'A', 'E', 'B', 'F#', 'Db', 'Ab', 'Eb', 'Bb', 'F'],
  minor: ['a', 'e', 'b', 'f#', 'c#', 'g#', 'd#', 'bb', 'f', 'c', 'g', 'd'],
  roman: ['I', 'V', 'ii', 'vi', 'iii', 'vii°', '', '', '', '', '', 'IV'],
  function: ['Tonik', 'Dominant', 'Süpertonik', 'Altmedyan', 'Medyan', 'Çeken ses', '', '', '', '', '', 'Altdominant']
};

const notes=computed(()=>
{
  switch(selectedIndex.value) {
    case 7:
      return(
          {
            sound: ['GS2', 'GS2', 'C#5','D#4', 'F#2','G#2', 'A#4', 'C#5','D#4', 'F#2','G#2', 'A#4'],
            displayData: ['about C', 'about G ', 'about D', 'about A', 'about E', 'about B', 'about F#', 'about Db', 'about Ab', 'about Eb', 'about Bb', 'about F'],

            major: ['C', 'G', 'D', 'A', 'E', 'B', 'Gb', 'Db', 'Ab', 'Eb', 'Bb', 'F'],
            minor: ['a', 'e', 'b', 'f#', 'c#', 'g#', 'd#', 'bb', 'f', 'c', 'g', 'd'],
            roman: ['I', 'V', 'ii', 'vi', 'iii', 'vii°', '', '', '', '', '', 'IV'],
            function: ['Tonik', 'Dominant', 'Süpertonik', 'Altmedyan', 'Medyan', 'Çeken ses', '', '', '', '', '', 'Altdominant']
          })
      break;
    case 3:
      return(
          {
            sound: ['GS2', 'GS2', 'C#5','D#4', 'F#2','G#2', 'A#4', 'C#5','D#4', 'F#2','G#2', 'A#4'],
            displayData: ['about C', 'about G ', 'about D', 'about A', 'about E', 'about B', 'about F#', 'about Db', 'about Ab', 'about Eb', 'about Bb', 'about F'],

            major: ['C', 'G', 'D', 'A', 'E', 'B', 'Gb', 'C#', 'G#', 'Eb', 'Bb', 'F'],
            minor: ['a', 'e', 'b', 'f#', 'c#', 'g#', 'd#', 'bb', 'f', 'c', 'g', 'd'],
            roman: ['I', 'V', 'ii', 'vi', 'iii', 'vii°', '', '', '', '', '', 'IV'],
            function: ['Tonik', 'Dominant', 'Süpertonik', 'Altmedyan', 'Medyan', 'Çeken ses', '', '', '', '', '', 'Altdominant']
          })
      break;
    default:
return(
    {
      sound: ['G#2', 'A#4', 'C#5','D#4', 'F#2','G#2', 'A#4', 'C#5','D#4', 'F#2','G#2', 'A#4'],
          displayData: ['about C', 'about G ', 'about D', 'about A', 'about E', 'about B', 'about F#', 'about Db', 'about Ab', 'about Eb', 'about Bb', 'about F'],

        major: ['C', 'G', 'D', 'A', 'E', 'B', 'F#', 'Db', 'Ab', 'Eb', 'Bb', 'F'],
        minor: ['a', 'e', 'b', 'f#', 'c#', 'g#', 'd#', 'bb', 'f', 'c', 'g', 'd'],
        roman: ['I', 'V', 'ii', 'vi', 'iii', 'vii°', '', '', '', '', '', 'IV'],
        function: ['Tonik', 'Dominant', 'Süpertonik', 'Altmedyan', 'Medyan', 'Çeken ses', '', '', '', '', '', 'Altdominant']
    })
      // code block
  }





})


/*

const notes ={
  major: ['C', 'G', 'D', 'A', 'E', 'B', 'F#', 'Db', 'Ab', 'Eb', 'Bb', 'F'],
  minor: ['a', 'e', 'b', 'f#', 'c#', 'g#', 'd#', 'bb', 'f', 'c', 'g', 'd'],
  roman: ['I', 'V', 'ii', 'vi', 'iii', 'vii°', 'IV#', 'bII', 'bVI', 'bIII', 'bVII', 'IV'],
  function: ['tonic', 'dominant', 'supertonic', 'submediant', 'mediant', 'leading tone', 'raised subdominant', 'neapolitan', 'submediant', 'mediant', 'subtonic', 'subdominant']
}


*/


import * as Tone from 'tone'
let sampler: Tone.Sampler

function initiateSampler() {

  //toneJsLib.onload = () => {
   sampler = new Tone.Sampler({
      urls: {
        C3: 'C3.mp3',
        'D#3': 'Ds3.mp3',
        'F#3': 'Fs3.mp3',
        A4: 'A4.mp3',
        'G#2': 'GS2.mp3',
        'A#4': 'A#4.mp3',
        'C#5': 'C#5.mp3',
        C4: 'C4.mp3',
        'D#4': 'Ds4.mp3',
        'F#4': 'Fs4.mp3',
      },
      release: 0.5,
      baseUrl: '/',
    }).toDestination();

    console.log("loaded")
  //};
}
const playingIndex=ref<number | null>(null)
// notes each .25s
function play(notex: number[], isMajor = true) {
  const now = Tone.now();
  notex.forEach((note, idx, arr) => {
    // Schedule each note to play
    setTimeout(() => {
      playingIndex.value = note;
      sampler.triggerAttackRelease(
        notes.value.sound[note],
        '8n'
      );
      
      // Clear the playing indicator after a short delay
      setTimeout(() => {
        if (playingIndex.value === note) {
          playingIndex.value = null;
        }
      }, 500);
    }, idx * 500);
  });

}


// Current rotation angles for each wheel
const outerRotation = ref(0);
const middleRotation = ref(0);
const innerRotation = ref(0);

// Current selected indices
const selectedIndex = ref(0);

// Function to rotate the wheels
function rotateToIndex(index: number) {
  // Calculate the rotation needed (each segment is 30 degrees)
  const targetRotation = -index * 30;
  
  // Update rotations
  outerRotation.value = targetRotation;
  middleRotation.value = targetRotation;
  innerRotation.value = targetRotation;
  
  // Update selected index
  selectedIndex.value = index;
}

// Function to get the color for a specific function
function getFunctionColor(func: string): string {
  const colorMap: Record<string, string> = {
    'Tonik': '#A83232',        // Red
    'Dominant': '#D4AF37',     // Gold
    'Süpertonik': '#32A852',   // Green
    'Altmedyan': '#3273A8',   // Blue
    'Medyan': '#A832A8',      // Purple
    'Çeken ses': '#A87D32', // Brown
    'Altdominant': '#E91E63',  // Pink
  };
  

  
  return colorMap[func] || '#555555';
}

// Calculate position of note on staff
function calculateNotePosition(noteValue: number) {
  // Map MIDI note values to staff positions
  const basePosition = 60; // Middle C position
  return ((noteValue - basePosition) * 3.5) + 60; // 3.5px per semitone, 60px baseline
}

// Initialize
onMounted(() => {
  initiateSampler();
  rotateToIndex(0); // Start with C as the tonic


});
</script>

<template>
  <!-- :style="`transform: rotate(${outerRotation}deg)`"--> 
  <div class="circle-of-fifths-container ">
    <Card class="mb-4">
      <div class="flex flex-col gap-2">
        <div>Selected Key: {{ notes.major[selectedIndex] }} major / {{ notes.minor[selectedIndex] }} minor</div>
      </div>
      <Button @click="play([4,3,4,4,3])">Play</Button>
     note playing index: {{ playingIndex }}
<br>

{{ notes.displayData[selectedIndex] }}

    </Card>
    
    <div class="circle-container">
      <svg viewBox="0 0 1000 1000" class="circle-of-fifths">
        <!-- Outer wheel (functions) -->
        <g class="wheel outer-wheel" >
          <g v-for="(func, index) in notes.function" :key="`func-${index}`">
            <path 
              :d="`M500,500 L${500 + 450 * Math.sin(index * Math.PI/6)},${500 - 450 * Math.cos(index * Math.PI/6)} A450,450 0 0,1 ${500 + 450 * Math.sin((index+1) * Math.PI/6)},${500 - 450 * Math.cos((index+1) * Math.PI/6)} Z`"
              :fill="getFunctionColor(func)"
              :class="{ 'selected-segment': index === selectedIndex }"
              @click="rotateToIndex(index)"
              fill-opacity="0.8"
              stroke="#1a2e3b"
              stroke-width="2"
            />
            
            <!-- Function name text (positioned along the outer edge) -->
            <text 
              v-if="func"
              :x="500 + 400 * Math.sin((index + 0.5) * Math.PI/6)" 
              :y="500 - 400 * Math.cos((index + 0.5) * Math.PI/6)"
              text-anchor="middle"
              dominant-baseline="middle"
              fill="#ffffff"
              font-size="18"
              font-weight="bold"
              :transform="`rotate(${index * 30}, ${500 + 400 * Math.sin((index + 0.5) * Math.PI/6)}, ${500 - 400 * Math.cos((index + 0.5) * Math.PI/6)})`"
              class="function-text"
            >
              {{ func }}
            </text>
            
            <!-- Roman numeral text -->
            <text 
              v-if="notes.roman[index]"
              :x="500 + 350 * Math.sin((index + 0.5) * Math.PI/6)" 
              :y="500 - 350 * Math.cos((index + 0.5) * Math.PI/6)"
              text-anchor="middle"
              dominant-baseline="middle"
              fill="#ffffff"
              font-size="48"
              font-weight="bold"
              :transform="`rotate(${index * 30}, ${500 + 350 * Math.sin((index + 0.5) * Math.PI/6)}, ${500 - 350 * Math.cos((index + 0.5) * Math.PI/6)})`"
              class="roman-text"
            >
              {{ notes.roman[index] }}
            </text>
          </g>
        </g>
        
        <!-- Middle wheel (major keys) -->
        <g class="wheel middle-wheel" :style="`transform: rotate(${middleRotation}deg)`">
          <circle cx="500" cy="500" r="300" fill="#0a3b4a" stroke="#1a2e3b" stroke-width="2" />
          <g 
          v-for="(note, index) in notes.major" :key="`major-${index}`"
             @click="rotateToIndex(index)">
            <path class="key-segment"
              :d="`M500,500 L${500 + 300 * Math.sin(index * Math.PI/6)},${500 - 300 * Math.cos(index * Math.PI/6)} A300,300 0 0,1 ${500 + 300 * Math.sin((index+1) * Math.PI/6)},${500 - 300 * Math.cos((index+1) * Math.PI/6)} Z`"
              :fill="
             
            index === selectedIndex ? getFunctionColor(notes.function[index]) : '#0a3b4a'"
              stroke="#1a2e3b"
              stroke-width="2"
            />
            <text  
              :x="500 + 250 * Math.sin((index + 0.5) * Math.PI/6)" 
              :y="500 - 250 * Math.cos((index + 0.5) * Math.PI/6)"
              text-anchor="middle"
              dominant-baseline="middle"
              :fill="index === playingIndex ? '#FFFF00' : (index === selectedIndex ? '#ffffff' : '#aaccdd')"
              font-size="60"
              font-weight="bold"
              :transform="`rotate(${index * 30}, ${500 + 250 * Math.sin((index + 0.5) * Math.PI/6)}, ${500 - 250 * Math.cos((index + 0.5) * Math.PI/6)})`"
              class="major-text"
              :class="{ 'playing': index === playingIndex }"
            >
              {{ note }}
            </text>
          </g>
        </g>
        
        <!-- Inner wheel (minor keys) -->
        <g class="wheel inner-wheel" :style="`transform: rotate(${innerRotation}deg)`">
          <circle cx="500" cy="500" r="180" fill="#051a24" stroke="#1a2e3b" stroke-width="2" />
          <g v-for="(note, index) in notes.minor" :key="`minor-${index}`"
             @click="rotateToIndex(index)">
            <path 
              :d="`M500,500 L${500 + 180 * Math.sin(index * Math.PI/6)},${500 - 180 * Math.cos(index * Math.PI/6)} A180,180 0 0,1 ${500 + 180 * Math.sin((index+1) * Math.PI/6)},${500 - 180 * Math.cos((index+1) * Math.PI/6)} Z`"
              fill="#051a24"
              stroke="#1a2e3b"
              stroke-width="1"
              class="key-segment"
            />
            <text 
              :x="500 + 140 * Math.sin((index + 0.5) * Math.PI/6)" 
              :y="500 - 140 * Math.cos((index + 0.5) * Math.PI/6)"
              text-anchor="middle"
              dominant-baseline="middle"
              :fill="index === selectedIndex ? '#ffffff' : '#778899'"
              font-size="40"
              font-weight="bold"
              :transform="`rotate(${index * 30}, ${500 + 140 * Math.sin((index + 0.5) * Math.PI/6)}, ${500 - 140 * Math.cos((index + 0.5) * Math.PI/6)})`"
              class="minor-text"
            >
              {{ note }}
            </text>
          </g>
        </g>
        
        <!-- Center circle -->
        <circle cx="500" cy="500" r="80" fill="#021218" stroke="#1a2e3b" stroke-width="2" />
      </svg>
    </div>
  </div>
</template>

<style>
.circle-of-fifths-container {
    background-color: #ccdce2;

  height:100vh;
  width: 100vh;
  margin: 0 auto;
  padding: 2rem;
}

.circle-container {
  position: relative;
  width: 100%;
  max-width: 700px;
  margin: 0 auto;
}

.circle-of-fifths {
  width: 100%;
  height: auto;
  border-radius: 50%;
}

.wheel {
  transform-origin: center;
  transition: transform 0.5s ease-in-out;
}

.key-segment:hover {
  opacity: 0.8;
  cursor: pointer;
}

.selected-segment {
 /* stroke: #ffffff;
  stroke-width: 3;*/
}

text {
  user-select: none;
}

.function-text {
  font-family: 'Arial', sans-serif;
  letter-spacing: 0.5px;
  font-size: 16px;
}

.roman-text {
  font-family: 'Times New Roman', serif;
  font-weight: bold;
}

.major-text {
  font-family: 'Arial', sans-serif;
  font-weight: bold;
}

.minor-text {
  font-family: 'Arial', sans-serif;
  font-style: italic;
}

.playing {
  animation: pulse 0.5s infinite alternate;
}

@keyframes pulse {
  from { opacity: 0.7; }
  to { opacity: 1; }
}
</style>
