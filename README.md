# Schemaorg Dockerfile Parser

![https://github.com/openschemas/spec-template/raw/master/img/hexagon_square_small.png](https://github.com/openschemas/spec-template/raw/master/img/hexagon_square_small.png)

This is an example of using schema.org to label a Dockerfile. Specifically, we will
walk through examples that use already defined schema.org specifications, and other
examples that use [specifications for containers](https://www.github.com/openschemas/spec-container)
 that are under development. The goal is to demonstrate utility in being able to 
label a Dockerfile (a container recipe) and then different ideas for how this could be done.


## Why should I care?

Labeling our containers and container recipes with schema.org metadata means that
we can serve it alongside the containers, and have the properties indexed by Google Search.
This means that regardless of where your recipe is hosted (a registry or Github pages)
your container and recipes will (finally) be discoverable.

## What examples are here?

Toward this goal, we are going to walk through several examples representing a Dockerfile
as each of:

 - [ContainerRecipe](ContainerRecipe)
 - [SoftwareSourceCode](SoftwareSourceCode)

Each folder above includes a script to extract (`extract.py`), 
a recipe to follow (`recipe.yml`), and the specification in yaml format (in the 
case of a specification not served by production schema.org). When you view
the [github pages](https://openbases.github.io/extractor-dockerfile) of this 
repository served by the master branch, you will see that the `index.html` 
in each folder serves a simple page to show the extracted metadata. The 
[Dockerfile](Dockerfile) is the recipe that we aim to describe using schema.org,
and is for the container [toasterlint/storjshare-cli](https://hub.docker.com/r/toasterlint/storjshare-cli/) on Docker Hub. I chose it pretty randomly because it started with
"toast."


# Usage

Before running these examples, make sure you have installed the module (and note
this module is under development, contributions are welcome!)

```bash
pip install schemaorg
```

To extract a recipe for a particular datatype, just run the `extract.py` file
in the corresponding folder. You can look at any of the extractors to get a gist
of what we do to generate the final metadata for Github pages. Generally we:

 1. Read in a specific version of the *schemaorg definitions* provided by the library
 2. Read in a *recipe* for a template that we want to populate (e.g., google/dataset)
 3. Use helper functions provided by the template (or our own) to *extract*
 4. Extract, *validate*, and generate the final dataset

The goal of the software is to provide enough structure to help the user (typically a developer)
but not so much as to be annoying to use generally.

## What are the files in each folder?

### recipe.yml Files

If I am a provider of a service and want my users to label their data for my service,
I need to tell them how to do this. I do this by way of a recipe file, in each
example folder there is a file called `recipe.yml` that is a simple listing of required fields defined for the entities that are needed. For example, the [recipe.yml](SoftwareSourceCode/recipe.yml) in the "SoftwareSourceCode" folder tells the parser that we need to define
properties for "SoftwareSourceCode" and an Organization or Person. For example.
with the [schemaorg](https://www.github.com/openschemas/schemaorg) Python module 
I can learn that the "SoftwareSourceCode" definition has 121 properties, 
but the recipe tells us that we only need a subset of those
properties for a valid extraction.

### specification.yml

The specification.yml file is only provided in the [ContainerRecipe](ContainerRecipe)
folder because this isn't a production specification provided by schema.org. For
those that are published (e.g., SoftwareSourceCode and Dataset) the definitions are
provided in the python module.

### extractor.py

This is the code snippet that shows how you extract metadata and use the 
[schemaorg](https://www.github.com/openschemas/schemaorg) Python module
to generate the final template page. This file could be run in multiple places!

 - In a continuous integration setup so that each change to master updates the Github Pages metadata.
 - Using a tool like [datalad](https://datalad.org) that allows for version control of such metadata, and definition of extractors (also in Python).
 - As a Github hook (or action) that is run at any stage in the development process.
 - Rendered by a web server that provides Container Recipes for users that should be indexed with Google Search (e.g., Singularity Hub).

Check out any of the subfolders for:

 - [ContainerRecipe](ContainerRecipe)
 - [SoftwareSourceCode](SoftwareSourceCode)

*under development*