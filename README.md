#Caffeine

Caffeine is a set of SASS mixins and functions ready to use for projects. First versions of Caffeine were embedded as \_system definitions in [Melange](http://melange.io). As the mixins and functions grow, they are seperated from Melange in order to use them freely.

## Installation
Caffeine can directly install your project by copying the contents of lib folder to your assets, and [npm](https://www.npmjs.org/) or [bower](http://bower.io) packages. Just run,
with npm (_npm package renamed to `kafein` due to name conflicts_)

```
npm install kafein
```

or with bower

```
bower install caffeine
```

After copying the files, at the top of your SASS file add the following line
```
@import "<path-to-caffeine>/caffeine";
```