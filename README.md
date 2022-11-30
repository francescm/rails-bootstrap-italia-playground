# Create rails7 project with bootstrap-italia leveraging esbuild

    $ rails new -j esbuild -c sass bootstrap-italia-playground
    $ cd bootstrap-italia-playground
    $ yarn add bootstrap-italia

please do not add ```bootstrap```: it is 
already a ```bootstrap-italia```'s dependency.

## edit package.json

Resulting file after edits should be:

    {
    "name": "app",
    "private": "true",
    "dependencies": {
    "@hotwired/stimulus": "^3.1.1",
    "@hotwired/turbo-rails": "^7.2.4",
    "@popperjs/core": "^2.11.6",
    "bootstrap-italia": "^2.0.9",
    "esbuild": "^0.15.16",
    "sass": "^1.56.1"
    },
    "scripts": {
        "build": "esbuild app/javascript/*.* --bundle --sourcemap --outdir=app/assets/builds --public-path=assets",
        "build:css": "sass ./app/assets/stylesheets/application.sass.scss:./app/assets/builds/application.css --no-source-map --load-path=."
        }
    }

please note ```--load_path=.``` on ``build:css`` task (it's a fork from 
instructions received from the console).

## edit application.js

Edit ```app/javascript/application.js```:

    // Entry point for the build script in your package.json
    import "@hotwired/turbo-rails"
    import "./controllers"
    import * as bootstrap from "bootstrap"

## edit application.sass.scss

Edit ```app/assets/stylesheets/application.sass.scss```:

    // Entry point for your Sass build
    @import 'node_modules/bootstrap-italia/src/scss/bootstrap-italia';

The task:

     $ yarn build:css

should complete successfully with a few warnings.

## sprites.svg

     $ cp -v node_modules/bootstrap-italia/dist/svg/sprites.svg app/assets/images/

## test it

     $ rails g controller static test

download the index.html from 
[bootstrap-italia-playground](https://github.com/italia/bootstrap-italia-playground); 
remove from beginning to ```<body>``` (included); keep up to ```</footer>``` 
(included); rename it "test.html.erb" and copy into ./app/view/static.

Find ```/dist/svg/sprites.svg``` and replace with ```<%= image_url('sprites.svg') %>```

(with vim: 

     %s/\/dist\/svg\/sprites.svg/<%= image_url('sprites.svg') %>/g
     %s/{{ site.baseurl }}//g

)

     $ ./bin/dev

visit:

     http://localhost:3000/static/test




