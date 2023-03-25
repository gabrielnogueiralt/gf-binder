# gf-binder
[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gabrielnogueiralt/gf-binder/master)

## Playing with GF notebooks
Just press the icon above! Note that your changes or any new notebooks you create *will not be persistent*. That is, if you close your tab and go back later, all changes will be lost!

## Making your own shared notebooks
1. Make a fork of this repo
2. Clone your forked repo to your local machine
3. Install docker if you have not already (https://docs.docker.com/engine/install/)
4. Run the script `./run_locally.sh`
5. Copy and paste the link that the script outputs into your browser (it will be something like: http://127.0.0.1:8888/?token=XXXXXX)
6. Create any notebooks you want to share in the `notebooks/` folder (only stuff created in that folder will be mirrored back to your git repository, all other folders "live" only inside your docker container.)
7. Commit any notebooks you created to the git repository and push to GitHub.
8. Go to https://mybinder.org/ and enter the link to your forked GitHub repository.
9. (Optional) Change this readme and the link on the top logo badge so people can go directly to your notebooks from your GitHub repo.

## GF shell command reference

The examples are from the grammar in `notebooks/Example.ipynb`.

### parse
Short form `p`. Example:  
`p -lang=Eng "John loves Mary"`

### linearize
Short form `l`. Examples:  
`l love john mary`  
Linearizes the tree `love john mary` to all languages.

`p -lang=Eng "John loves Mary" | l`   
Parses the English sentence "John loves Mary", linearizes to all languages.

`p -lang=Eng "John loves Mary" | l -lang=Ger`  
Parses the English sentence "John loves Mary", linearizes to German.


`p -lang=Eng "John loves Mary" | l -treebank`   
Parses the English sentence "John loves Mary", linearizes to all languages in a *treebank* format:

```haskell
Example: love john mary
ExampleEng: "John loves Mary"
ExampleGer: "Johann liebt Maria"
```

### visualize_tree
Short form `vt`. Example:  
`p -lang=Eng "John loves Mary" | vt`

Visualises the abstract syntax tree for "John loves Mary". In the Jupyter notebook, you need to prefix this command with **view**:

**view** `p -lang=Eng "John loves Mary" | vt`

### visualize_parse
Short form `vp`. Example:  
`p -lang=Eng "John loves Mary" | vp`

Visualises the English parse tree for "John loves Mary". In the Jupyter notebook, you need to prefix this command with **view**:

**view** `p -lang=Eng "John loves Mary" | vt`


### generate_random
Short form `gr`. Generates a random tree. Examples:

`gr -number=5 | l`  
Generates 5 random trees (of start category), linearizes to all languages.

`gr -cat=NP`  
Geneerates a random tree of category NP.

### generate_trees
Short form `gt`. Generates all trees of the grammar (up to certain depth). 

Examples:

`gt -cat=NP | l`  
Generates all trees of type NP, linearizes them in all languages.

`gt | l -lang=Eng`  
Generates all trees (of start category), linearizes them in English.



