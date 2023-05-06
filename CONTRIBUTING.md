# Contributing


Contributions are welcome, and they are greatly appreciated! Every little bit
helps, and credit will always be given.

- Report bugs, request features or submit feedback as a [GitHub Issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues).
- Make fixes, add content or improvements using [GitHub Pull Requests](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)

Ready to contribute? Here's a quick guide that was modified from the eScience Hackweek template.



## Contributing website changes

To build our website, we need specific Python packages which are managed with the `conda` and `mamba` tools. If you already do not already have those tools installed, we recommend using the [Mambaforge Installer](https://github.com/conda-forge/miniforge#mambaforge):


1. Fork this hackweek's website repo on GitHub.

1. Clone your fork locally:

    ```sh
    git clone {{website_url}}.git
    cd jupyterbook-template
    ```

1. Create a branch to add your changes:

    ```sh
    git checkout -b name-of-your-bugfix-or-feature
    ```

1. Create and activate the "hackweek" conda environment.

   __NOTE__: If you're running linux or Windows use `conda/conda-linux-64.lock.yml`
    ```sh
    mamba env create --name hackweek --file conda/conda-osx-64.lock.yml
    mamba activate hackweek
    ```
   __NOTE__: If you want to add packages or change the environment,
    you must follow the procedure outlined in [./conda/README.md](./conda/README.md).

1. Make your desired changes and build the book locally

    ```sh
    ./scripts/build_resources.sh
    ```
   __NOTE__: to preview the changes open `book/build/html/index.html`

1. Push your branch to GitHub when you're ready:

    ```sh
    git add .
    git commit -m "Your detailed description of your changes."
    git push origin name-of-your-bugfix-or-feature
    ```

1. Open a pull request through the GitHub website: {{website_url}}


## Contributing tutorials

If you're adding a new Jupyter Notebook Tutorial, please first take a look at [our guide on creating tutorials](https://uwhackweek.github.io/hackweeks-as-a-service/resources/tutorial-resources.html).

When adding a new `.ipynb` file under `book/tutorials` be sure to:

  1. Add an entry to the table of contents `book/_toc.yml`

  1. "Clear all Outputs" before saving. This keeps the book source code small, but outputs are still built for the HTML webpage by Jupyter Book.


## Releasing new template versions
Before using this template for events, make a git tag and GitHub Release. We follow a [calendar versioning scheme](https://calver.org), so tags are a date like `2021.05.05`. Don't forget to update the [Changelog](./CHANGELOG.md)!
