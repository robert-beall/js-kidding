# Spiderworks
This is a knowledge-hub repo for web basics (JavaScript, HTML, CSS, etc.). It's name is inspired by the Lockheed Skunkworks, with spider referring to web technologies.

## Purpose
While I have a good handle on JavaScript as a whole, there are always unfamiliar concepts to explore.

## Structure
Unlike many other knowledge-hub repos, this one will bundle the notes and lab sections together. Individual concepts will have dedicated directories, with each directory containing a `README.md` file for notes and a `lab.js` file for code examples. Directories can contain subdirectories for related concepts.

I will be using the [mdn web docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript) as a guide. 

## How to Run
This project uses [node.js](https://nodejs.org/) as a platform for running code examples from the command line. Where a JavaScript concept only applies to client-side scripting, a note will be made and a simple `.html` file will be used to run `lab.js` instead. The HTML file can be viewed in the browser, with JavaScript code outputting either to the console or the page itself.

For node-compatible files, run the following in the file directory:

```
node <filename>.js
```
