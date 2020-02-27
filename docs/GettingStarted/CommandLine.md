# Quick Started for Command Line Tool
> This quick started guide assumes the [setup](GettingStarted/Setup) page is read and the environment is prepared.

## User Story
For a software development, defining the user story is the first thing. Our quick started user story is this.
> My current mapping software needs to work with ShapeFile. But I need to explore what the ShapeFile looks like, such as what shape type it is, how many features it includes and what the fields it has. In my development, I often switch my laptop between home (Windows) and office (macOS), so I want a cross platform light weighted tool to help me out those kind of requirements.

## Let's Get Started
Open your terminal and follow the commands below:

```bash
mkdir quick-started-cmd
cd quick-started-cmd
yarn init -y
yarn add ginkgoch-map
touch main.js
```

At this step, we already setup a basic command line tool project layout. To make it simple, I will avoid to involve the ES6 or TypeScript, but it is actually compatible with those two. I will introduce the ES6 or TypeScript integration later.

Source Code:
```javascript
const fs = require('fs');
const G = require('ginkgoch-map').default.all;

function main() {
    // make sure shapefile is specified
    if (process.argv.length < 3) {
        console.info('Specified a shapefile path parameter');
        return;
    }

    // make sure the shapefile does exist
    let filePath = process.argv[2];
    if (!fs.existsSync(filePath)) {
        console.error(`File ${filePath} doesn't exist`);
        return;
    }

    // create a shapefile instance
    let source = new G.Shapefile(filePath);
    source.open();
    
    // get shapefile header and print it
    let header = source.header();
    for(let key of Object.keys(header)) {
        console.log(`${key}\t\t${header[key]}`);
    }
}

main();
```

Then execute command `node main.js [a shapefile path]` will print the header information.
```bash
fileCode: 9994
fileLength: 3813840
version: 1000
fileType: 5
minx: -20037508.23146975
miny: -20037508.231469806
maxx: 20037508.23146975
maxy: 18418382.328923147
```

Get to know more about our API to you can do more powerful functions.

Here is a complete command tool for shapefile [node-shapefile-cli](https://github.com/ginkgoch/node-shapefile-cli) for your continue reading.