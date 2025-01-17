```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Idea Generator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
    }
    #output {
      font-size: 1.5rem;
      margin-top: 20px;
    }
    button {
      padding: 10px 20px;
      font-size: 1rem;
    }
  </style>
</head>
<body>
  <h1>Idea Generator</h1>
  <button id="generateButton">Generate Idea</button>
  <div id="output"></div>

  <script src="ideaGenerator.js"></script>
</body>
</html>
```html

```javascript
// Arrays of adjectives and nouns. Feel free to change these!
const adjectives = [
  "creative",
  "shiny",
  "mysterious",
  "fuzzy",
  "colorful",
  "wild",
  "brilliant",
  "quirky",
  "dynamic",
  "whimsical",
];

const nouns = [
  "adventure",
  "gadget",
  "creature",
  "planet",
  "idea",
  "machine",
  "character",
  "challenge",
  "puzzle",
  "story",
];

// Function to get a random element from an array
function getRandomElement(array) {
  return array[Math.floor(Math.random() * array.length)];
}

// Function to generate and display an idea
function generateIdea() {
  const adj1 = getRandomElement(adjectives);
  const adj2 = getRandomElement(adjectives);
  const noun = getRandomElement(nouns);

  // Ensure the adjectives are not the same
  const finalAdj2 = adj1 === adj2 ? getRandomElement(adjectives) : adj2;

  const idea = `${adj1} ${finalAdj2} ${noun}`;
  document.getElementById("output").textContent = idea;
}

// Attach event listener to the button
document.getElementById("generateButton").addEventListener("click", generateIdea);
```javascript
