# Neutralino-To-Do 
Non Official To Do List App example for NeutralinoJS + VueJS.

## Preview
To preview this app, you should install @neutralinojs/neu (Neutralino Command Line Tool) first.
```
npm install -g @neutralinojs/neu
```
Also, it will need the client libaray of NeutralinoJS:
```
neu update
```

Then run develop mode:
```
neu run
```
## Develop
All of the project's main files is in "vue-src" folder.
```
cd ./vue-src
yarn install
yarn dev
```

## Build
Before building App, you should build the vue files first:
```
cd ./vue-src
yarn build
```
Then back to the top folder:
```
cd ..
neu build
```


