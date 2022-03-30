# Sematic Versioning
Most projects that other projects depend on issue a _version number_ with every release. Version numbers serve many purposes, and one of the most important of them is to ensure that software keeps working. The exact meaning of each one varies between projects, but one relatively common standard is [_semantic versioning_](https://semver.org/). With semantic versioning, every version number is of the form: major.minor.patch. The rules are:
- If a new release is just about bug fix without any change in the API, increase the patch version.
- If you _add_ to your API in a backwards-compatible way, increase the minor version.
- If you change the API in a non-backwards-compatible way, increase the major version.

Under this scheme, version numbers and the way they change convey meaning about the underlying code and what has been modified from one version to the next.

This already provides some major advantages. Now, if my project depends on your project, it _should_ be safe to use the latest release with the same major version as the one I built against when I developed it, as long as its minor version is at least what it was back then. 
