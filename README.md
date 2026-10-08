# NC Manager Guides

Source for [guides.ncmanager.org](https://guides.ncmanager.org).

Built with [Jekyll](https://jekyllrb.com/) and the [USWDS Jekyll theme](https://github.com/18F/uswds-jekyll).

Deployed to GitHub Pages automatically on every push to `main` (`.github/workflows/pages.yml`).

To run locally:

  ```sh
  bundle install
  bundle exec jekyll serve   # http://localhost:4000
  bundle exec rake           # build + check links with html-proofer
  ```
