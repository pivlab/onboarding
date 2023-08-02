# Code review checklist

**Pride:** We expect lab members to sign their code via commits attributable to a user.
Each commit must be attributed to a recognized user.

**Licensing:** A LICENSE file is in the root of the repository.

**Using others code:** Code taken from elsewhere is properly acknowledged and compatible with the license.

**Style guide:** Python code follows [PEP 8](https://www.python.org/dev/peps/pep-0008).
R code follows [Google's R Style Guide](https://google.github.io/styleguide/Rguide.xml).
JavaScript code follows [Google's JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml).
HTML and CSS follow [Google's HTML/CSS Style Guide](https://google.github.io/styleguide/htmlcssguide.xml).
We expect that each person runs a linter (if you're not sure -- [ask](https://pivlab.slack.com/)) as part of their development environment.
We provide instructions on how to automate the linting process [here](linter_install_tutorial.md)

**Variable and function names:** Variable names are descriptive and interpretable to someone looking at this code for the first time (e.g. not `a`, `b`, `x`, etc.).

**File commenting:** Each file has a comment at the top to broadly describe its function and how it is expected to be used (e.g. imported, run from command line, both).

**Function comments:** Each function has a docstring which reports the computation that it intends to implement, its arguments, and its return value(s).

**In-line commenting:** At least 2 spaces are placed between in-line comments (`#`) and source code.

**Imports:** All trivial imports are at the top of the file.

**Column length:** The code should be readable in a text editor with a reasonable format as well as the GitHub interface.
This means that there are no excessively long lines.
We strongly recommend that repository maintainers select a maximum line length for code of 80 or 100 characters and that this be specified in a contributors document for the repository.
Plain text, markdown, and other text-based formats can alternatively be broken at sentences.
This rule is already covered well in [PEP 8](https://www.python.org/dev/peps/pep-0008/#maximum-line-length) but called out here to clarify that we apply it to more than Python code.
One reason for this is to aid in readability of `diff` output when performing code reviews.

**Whitespace:** There is no unnecessary whitespace.

**Code with constants** Any constants are specified at the beginning of the file.

**Code that uses a random seed [special case of constants]** Code that uses a random seed is reproducible.
This means that the seed can be set *and* a default value is specified.

**API error handling** APIs should catch and handle anticipated errors (e.g. key doesn't exist, type mismatch in lookup) by identifying the source of the error (e.g. lookup failed with PK=XYZ) to the caller with as much precision as possible.
