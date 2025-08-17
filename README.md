WebVOWL [![Build Status](https://travis-ci.org/VisualDataWeb/WebVOWL.svg?branch=master)](https://travis-ci.org/VisualDataWeb/WebVOWL)
=======

> [!CAUTION]
> The URL https://visualdataweb.org/ is not owned bei VisualDataWeb anymore! Not related to WebVOWL anymore.

This repository was ported from an internal SVN repository to Github after the release of WebVOWL 0.4.0. Due to cleanups with `git filter-branch`, the commit history might show some strange effects.

Run Using Docker
------------
Make sure you are inside `WebVOWL` directory and you have docker installed. Run the following command to build the docker image:

`docker build . -t webvowl:v1`

Run the following command to run WebVOWL at port 8080. 

`docker-compose up -d` 

Visit [http://localhost:8080](http://localhost:8080) to use WebVOWL.

Requirements
------------

Node.js for installing the development tools and dependencies.


Development setup
-----------------

### Simple ###
1. Download and install Node.js from http://nodejs.org/download/
2. Open the terminal in the root directory
3. Run `npm install` to install the dependencies and build the project
4. Edit the code
5. Run `npm run-script release` to (re-)build all necessary files into the deploy directory
6. Run `serve deploy/` to run the server locally, by installing serve by using `npm install serve -g`.

Visit [http://localhost:3000](http://localhost:3000) to use WebVOWL.

### Advanced ###
Instead of the last step of the simple setup, install the npm package `grunt-cli` globally with
`npm install grunt-cli -g`. Now you can execute a few more advanced commands in the terminal:

* `grunt` or `grunt release` builds the release files into the deploy directory
* `grunt package` builds the development version
* `grunt webserver` starts a local live-updating webserver with the current development version
* `grunt test` starts the test runner
* `grunt zip` builds the project and puts it into a zip file


Additional information
----------------------

To export the VOWL visualization to an SVG image, all css styles have to be included into the SVG code.
This means that if you change the CSS code in the `vowl.css` file, you also have to update the code that
inlines the styles - otherwise the exported SVG will not look the same as the displayed graph.

The tool which creates the code that inlines the styles can be found in the util directory. Please
follow the instructions in its [README](util/VowlCssToD3RuleConverter/README.md) file.




# OWL to VOWL JSON Conversion Guide

This guide explains how to convert an `.owl` ontology file to a `.vowl.json` file using OWL2VOWL manually.

## Steps

### Build OWL2VOWL
Run the following command to build OWL2VOWL:

```bash
mvn clean install -DskipTests
```

### Prepare your .owl file
Move your ontology file to the `ontologies/` folder:

```
OWL2VOWL/ontologies/oeo-full.owl
```

### Run the conversion
Execute the following command to convert the `.owl` file to `.vowl.json`:

```bash
java --add-opens java.base/java.lang=ALL-UNNAMED \
    -cp "target/OWL2VOWL-0.3.7.jar:target/dependency/*" \
    de.uni_stuttgart.vis.vowl.owl2vowl.ConsoleMain \
    -file ontologies/oeo-full.owl \
    -output ontologies/oeo-full.vowl.json
```

### Common Error & Fix

**Error:**
```
ERROR StatusLogger Log4j2 could not find a logging implementation. Please add log4j-core to the classpath.
```

**Fix:**
This is a logging warning. The conversion still works, and the `.vowl.json` file is created. No additional steps are needed unless you want proper Log4j2 logging.

### Locate the output file
The JSON file will be in the same folder:

```
OWL2VOWL/ontologies/oeo-full.vowl.json
```

### Use the JSON
Open the `.vowl.json` file in a VOWL viewer to visualize your ontology.

