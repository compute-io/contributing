Compute.io
==========

> A contributing guide.


## Table of Contents

1. 	[Overview](#overview)
1. 	[Joining](#joining)
1. 	[Style Guide](#style-guide)
	-	[Type Checking](#type-checking)
1. 	[Getting Started](#getting-started)
1. 	[Tests](#tests)
1. 	[Workflow](#workflow)
1. 	[Versioning](#versioning)
1. 	[Licensing](#licensing)


## Overview

[Compute.io](https://github.com/compute-io) is a JavaScript computation library. Each library function is written as a standalone module which can be independently imported into any application or project.

Monolithic libraries can be beneficial in the sense that these libraries centralize a codebase. Rarely, however, does one require all library functionality. All too often an application requires only one library function; in which case, the rest of the library is unnecessary and leads to code bloat.

Compute.io is guided by three design principles: __composition__, __consistency__, and __rigor__.

* 	__Composition__: standalone modules encourage flexible composition (build your own compute), constrained design (do one thing; do one thing well), and restraint (only use what you need).

*	__Consistency__: all modules should look and feel as if written by a single author. A user should be free to focus on understanding algorithms without the distraction of arbitrary changes in style.

*	__Rigor__: testing and benchmarking should be priorities, not afterthoughts, and algorithms should be robust. Too many "numerical" libraries written in JavaScript implement poor algorithms, lack precision, and disregard performance.

What follows is a guide to contributing to [Compute.io](https://github.com/compute-io).


## Joining

Have a great idea for a compute module or want to become more involved? Awesome. :)

You have a couple of options...

1. 	Create a module linked to your personal Github account and submit a pull request for inclusion in the main [compute.io](https://github.com/compute-io/compute.io) library.
2. 	(__preferred option__) Ask an [owner](https://github.com/kgryte) to become a [compute-io](https://github.com/compute-io) member.

If you opt for option #1, note that all compute modules are expected to follow the same style guide described below.


## Style Guide

All compute modules should follow the same JavaScript [style guide](https://github.com/kgryte/javascript-style-guide). Users should not have to decode styles to read and understand what matters: algorithms.

Each module repository should include the following files which enforce style consistency:

*	`.editorconfig`: defines editor settings for [compatible](http://editorconfig.org/#download) editors, such as [Sublime Text](http://www.sublimetext.com/) and [Atom](https://atom.io/).
*	`.jshintrc`: defines [JSHint](http://www.jshint.com/docs/) behavior.

Any tasks, such as tests, examples, documentation generation, benchmarking, etc, should be invoked via a `Makefile` target.


#### Type Checking

You are __strongly encouraged__ to type check input arguments. While including checks leads to longer modules and more required tests, doing so helps define user expectations and allows users to more easily debug their code.

``` javascript
// Do:
function mmean( win ) {
	if ( typeof win !== 'number' || win !== win || win < 1 ) {
		throw new TypeError( 'win()::invalid input argument. Window size must be a positive integer.')
	}
	// Do something...
}

// Don't:
function mmean( win ) {
	// Do something...
}
```


## Getting Started

Contributors are encouraged to use the [Yeoman](http://yeoman.io/) module [generator](https://github.com/compute-io/generator-compute-io). The generator automates many aspects of module generation by creating a base scaffold from which to build a compute module.

A couple of notes...

1. 	Before using the generator, be sure to create a remote repository on [Github](https://github.com/compute-io). The generator will use the repository name to generate the remote URLs included in the `package.json` and `README.md`. Additionally, the generator takes care of setting the remote origin for the local Git repository, so you can begin pushing code to the [compute-io](https://github.com/compute-io) organization immediately after generation.
2. 	Before pushing code to Github, be sure to turn on continuous integration using [travis-ci](https://travis-ci.org/), and be sure to turn on code coverage using [coveralls](https://coveralls.io/). For the savvy, travis-ci can be [enabled](https://github.com/travis-ci/travis.rb#enable) from the command-line; just ensure that you sync first.


## Workflow

The following is a typical workflow when creating compute modules...

1. 	Create a repository within the Github [organization](https://github.com/compute-io/).
2. 	Enable [travis-ci](https://travis-ci.org/) and [coveralls](https://coveralls.io/).
3. 	Create a new local directory.
	
	``` bash
	$ mkdir <repo_name>
	$ cd <repo_name>
	```

4. 	Run the [generator](https://github.com/compute-io/generator-compute-io) and follow the prompts.
	
	``` bash
	$ yo compute-io
	```

	Note that the module name should follow the convention of the repository name being prefixed with `compute-`; .e.g., `compute-mean`, where `mean` is the repository name.

5. 	Open the project in your favorite text editor; e.g.,
	
	``` bash
	$ subl .
	```

6. 	Edit the `README.md`. Define the module's behavior, including example code. Consider this writing a module [specification](http://www.joelonsoftware.com/articles/fog0000000036.html).
7. 	Copy the example from the `README.md` to the examples file `./examples/index.js`. The executable example code may be modified for clarity and include additional cases, but should at least resemble the example provided in the `README.md`.
8. 	Update the `package.json` __keywords__. Consider this [search engine optimization](https://www.npmjs.org/doc/files/package.json.html).
	* 	If I were searching for this module, what search terms might I use?

9. 	Write tests against the `README.md` in `./test/test.js`.
	* 	What are the expected input arguments? 
	*	What is the output type?
	*	What is the expected behavior?
10. Implement the module in `./lib/index.js`.
11. Run the tests against the module.

	``` bash
	$ make test-cov
	```

12. Fix broken tests and achieve __100%__ code coverage.
13. Run the example code and confirm expected output.

	``` bash
	$ node ./examples/index.js
	```

14. Read through the module to ensure everything is correct (e.g., descriptions, code documentation, spelling, edge cases, etc).
15. Commit and push the code to the remote repository.

	``` bash
	$ git add -A
	$ git commit -a
	$ git commit
	$ git push origin master
	```

16. Visit the Github repository. Read the `README.md` and ensure that everything is correct.
17. After waiting for 1-2 minutes, [travis-ci](https://travis-ci.org/) should have attempted to build the module. Confirm that the build succeeded (the `README.md` badge should transition from `pending` to `passing`), and confirm that [coveralls](https://coveralls.io/) received a code coverage report (the `README.md` badge should show the percent code coverage).
18. Inform an organization [owner](https://github.com/kgryte) that the module is ready and a candidate for publishing. An owner will subsequently review the module and suggest any changes which need to be made before publishing and inclusion in the main library.
19. Once the module has been green-lighted, publish the module to [NPM](https://npmjs.org).
	
	``` bash
	$ npm publish
	```

20. Create a new release by creating a new Git tag.

	``` bash
	$ git tag -a <major.minor.patch> -m "Initial release."
	$ git push origin <major.minor.patch>
	```

	where the `<major.minor.patch>` version should be the same version published to NPM.

21. If the module is stable (an owner will confirm this during the module review), bump the version to a stable status (e.g., `>=1.0.0`).

	``` bash
	$ npm version major -m "[UPDATE] bump version."
	$ git push origin master
	```

22. Publish the updated module to [NPM](https://npmjs.org).
	
	``` bash
	$ npm publish
	```

23. Create a new release and push to the remote repository.

	``` bash
	$ git tag -a <major.minor.patch> -m "[UPDATE] <release notes>."
	$ git push origin <major.minor.patch>
	```

	where, once again, the tagged release should match the version published to NPM.



## Versioning

Once published, the module should be versioned using [semantic versioning](http://semver.org/).
*	Any bug fixes should be *patches*.
*	Any new functionality which is __not__ API breaking should be communicated as a *minor* update; e.g., additional configuration options, etc.
*	Any modified or new functionality which is API breaking should be communicated as a *major* update.

Once a module is updated and its associated tests are passing, bump the version, push to the remote repository, and publish.

``` bash
$ npm version <major|minor|patch> -m "<message>"
# => returns the new semver version
$ git push origin master
$ npm publish
$ git tag -a <major.minor.patch> -m "[UPDATE] <release notes>."
$ git push origin <major.minor.patch>
```


## Tests

All modules __must__ instrument testing. Currently, [Mocha](http://mochajs.org/) is the preferred test framework with [Chai](http://chaijs.com) assertions. For code coverage, you are encouraged to use [Istanbul](https://github.com/gotwarlost/istanbul).

If you use the Compute.io [generator](https://github.com/compute-io/generator-compute-io), the above test modules are included along with stubbed test code. All modules should have __100%__ code coverage.




## Licensing

All modules should be licensed under an [MIT](http://opensource.org/licenses/MIT) license.
