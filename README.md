# DigiKAR: QGIS hands-on session, 11-12 July 2023
This is the repository for the companion website of the DigiKAR QGIS Tutorial that will be held at the Leibniz Institute for Regional Geography in Leipzig on July 11-12 2023.

You can access the official website at [icdar21-mapseg.github.io](https://digikar-qgis.github.io).

### How to contact the organizers?
You can **reach the organizers by email** at `digikar-qgis.rg2ug(at)aleeas.com``.

### How to update the website?
1. Install [MkDocs](https://www.mkdocs.org/) and [markdown-katex](https://gitlab.com/mbarkhau/markdown-katex) (to support math, please check this page for syntax documentation).  
   You can install the requirements using `pip install -r requirements.txt`.
2. Clone this repository.
3. Create/update/delete pages under `docs/`.
4. Run `mkdocs serve` and open up `http://127.0.0.1:8000/` in your browser to check the results.
5. When happy with your changes, then deploy your changes with `mkdocs gh-deploy` (no continuous integration for now).
6. Do not forget to commit your changes with `git add ...`, `git commit ...` and `git push`!

