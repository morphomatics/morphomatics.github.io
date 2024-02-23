# Morphomatics Website Repo

Repository for GitHub pages website of [Morphomatics](https://morphomatics.github.io).

The site is generated using [MkDocs](https://www.mkdocs.org/).
All source file are contained in the `/docs` folder.

To preview, edit or build the site from the sources you need to get a local copy the repository and a `python` environment set up with:
```bash
pip install mkdocs mkdocs-material mkdocs-material-extensions mkdocs-jupyter
```

The built-in dev-server allows you to preview your edits as you're writing it. It will even auto-reload and refresh your browser whenever you save changes. To start the server run
```bash
mkdocs serve
```
from the root of the repository. Once the server is up you should be able to see the previous in your browser at [http://127.0.0.1:8000/](http://127.0.0.1:8000/).

To build the site you need to do the following steps:
* run `mkdocs build` in the root folder
* copy all content from the generated `/site` folder into the root folder
* add to repo and push to GitHub
