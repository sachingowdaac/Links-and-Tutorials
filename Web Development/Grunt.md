----------------------------------
Grunt: The JavaScript Task Runner:
----------------------------------

	-	Task-based command line build tool for JavaScript projects which makes performing repetative, but neccesary, task trivial
	-	Validates css/html/JavaScript
	-	Minify or compress CSS/Javascript
	-	Compile CoffeeScript, TypeScript etc.,
	-	Compile less to CSS

	# Files required:

		-	package.json metadata file which discribes the project
			ex: package.json

				{
					"name" : "IntroductionToGrunt.js",
					"version" : "0.1.0",
					"devDependencies" : {
						"grunt" : "~0.4.1"
					}
				}

				Ref: https://www.nodejitsu.com/documentation/appendix/package-json/

		-	Gruntfile.js -> configure or define GRUNT tasks
			ex: Gruntfile.js

				'use strict';
				module.exports = function(grunt) {
					grunt.initConfig({
						pkg: grunt.file.readJSON('package.json')
					});
					uglify: {
				      options: {
				        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
				      },
				      build: {
				        src: 'src/<%= pkg.name %>.js',
				        dest: 'build/<%= pkg.name %>.min.js'
				      }
				    }

					// Load the plugin that provides the "uglify" task.
					grunt.loadNpmTasks('grunt-contrib-uglify');

					// Default task(s).
					grunt.registerTask('default', ['uglify']);
				};

			- 	Run in commandline
				$grunt taskName -v

	# Installation:

		-	First install node.js, install npm (node package manager), then install Grunt cli - command line interface, run,

			$npm install -g grunt-cli // here -g means global (to make sure grunt is available in all the folders in the system)

		-	then, install grunt to local directory by running,

			$npm install grunt --save-dev // Saves to local directory

	# Sample grunt example:

		Download 'grunt-contrib-clean' to use it, use npm to download, once downloaded it adds it to package.json automatically/we can add it
		manually, add this to Gruntfile.js

			use 'strict';

			module.exports = function(grunt) {
				grunt.initConfig({
					pkg: grunt.file.readJSON('package.json')
					clean: {
					output: ['ToBeCleaned/*']
					}
				)};

				grunt.loadNpmTasks('grunt-contrib-clean');

				grunt.registerTask('default', ['clean']);
			};

		-	run using, $grunt default -v // -v says verbose mode

	# Working with JavaScript:

		-	Validating JS with JSHint 	-> 	$npm install -grunt-contrib-jshint --save-dev

		-	Compressing JS with Uglify 	->	$npm install -grunt-contrib-uglify --save-dev , off/on mangling - feature to rename variables and 		methods to meaningfull names for development so that somebody can not interpret it easily.

		-	Cleaning folders and files with clean	->	$npm install -grunt-contrib-clean --save-dev

	# Using JSHint:

		-	Load and register it, specify in Gruntfile.js,
				grunt.loadNpmTasks('grunt-contrib-jshint'); then add it to registerTask
				grunt.registerTask('default', ['typescript', 'jshint']); -- these will run sequentially

		-	Then add the following entry to, grunt.initConfig({
				jshint: {
					options: {
						force: true // It forcefully continues, can be removed when below options are set
						'-W069': false, // failure due to the way typescript works with enums
						'W0004': false, // failure due to typescript inheritance
						ignores: ['path/to/js-file-to-ignore'], // Can be a multiple files seperated by comma
						reporterOutput: './jshint.txt' // Writing these erros to output file
					},
					files: ['./www/js/*.js']
				},
			});

	# Using uglify:

		-	Load and register it, specify in Gruntfile.js,
				grunt.loadNpmTasks('grunt-contrib-uglify');
				grunt.registerTask(default, ['typescript','uglify']);

		-	Add this entry to, grunt.initConfig({
				uglify: {
					development: {
						files: {
							'': ['']
							// which will compress one file in the root folder, multiple files can be added seperated by comma, or
							// The following files option can be used to scan the files as when they are added,
								files : [{
										expand: true,
										cwd: './www/js/',
										src: '**/*.js',
										dest: './www/js/'
									}]
						}
					},
					options: {
						mangle: false, // Keeps original variable names and method names intact
						compress: {
							drop_console: true // Which removes all the console.log() from the code, then compresses it
						},
						beautyfy: true // Helps in debugging the code, can be turned off before deploying
					}
				}
			});

	# Using clean:

		-	Load and register it,

				grunt.loadNpmTasks('grunt-contrib-clean');
				grunt.registerTask(default, ['clean', typescript','uglify']); 	// 	can be specified first, since we need clean up every time 																		building it freshly

		-	Add this entry to, grunt.initConfig({
				clean: {
					options: {

					},
					files: ['file/to/to/be/removed'],
					folders: ['folder/to/remove']
				}
			});

		-	A single task can be specified in the grunt command line, so that we can have control over the execution, ex: $grunt clean and with 	options, $grunt clean:folders

	# Working with HTML and CSS:

		-	Compressing HTML with contrib-htmlmin 	->	$npm install grunt-contrib-htmlmin, remove html comments, collapse white-space and tags, 	 remove redundant tag elements

		-	Validating HTML with htmlHint 			->	$npm install grunt-htmllhint, ensure unique tag id's, ensure valid attributes, tags are 	correct and valid

		-	Converting less to css with contrib-less ->	$npm install grunt-contrib-less, modify less variables, compress

		-	Validating CSS with contrib-csslint		->	$npm install grunt-contrib-csslint, configure rules (35 rules), use external config file

		-	Compress CSS with cssmin				->	$npm install grunt-contrib-cssmin, compress, minify single/mutiple or concat to one file

	# Using htmlHint:

		-	Add this entry to, grunt.initConfig({
				htmlhint: {
					templates: {
						options: {
							'attr-lower-case': true,
							'attr-value-not-empty': true,
							'tag-pair': true,
							'tag-self-close': true,
							'tagname-lowercase':true,
							'id-class-value': true,
							'id-class-unique': true,
							'src-not-empty': true,
							'img-alt-required': true
						},
						src: ['www/**/*.html']
					}
				}
			});

	# Using htmlmin:

		-	Add this entry to, grunt.initConfig({
				htmlmin: {
					dev: {
						options: {
							removeEmptyAttributes: true,
							removeEmptyElements: true,
							removeRedundantAttributes: true,
							removeComments: true,
							removeOptionalTags: true,
							collapseWhitespace: true
						},
						files: {
							'www/template/sample.min.html' : ['www/template/sample.html']
							// The following files option can be used to scan the files as when they are added,
								files : [{
										expand: true,
										cwd: './www/html/',
										dest: './www/html/',
										src: '[*.html]',
										ext: '.min.html',
										extDot: 'last'
									}]
						}
					}
				}
			});

	# Using less: Creates css from less

		-	Add this entry to, grunt.initConfig({
				less: {
					development: {
						options: {
							cleancss: true,
							compress: true,
							modifyVars: {
								"color-primary-dark-value": "NOT_BLUE" // value in a less file modified
							}
						},
						files: { "www/css/main.css" : "www/css/main.less" }
						// The following files option can be used to scan the files as when they are added,
								files : [{
										expand: true,
										cwd: './www/css/',
										dest: './www/css/',
										src: '[*.less]',
										ext: '.css',
										extDot: 'last'
									}]
					}
				}
			});

	# Using csslint: By default all the rules are enabled, to disable some, use external config file as specified in options using, csslintrc 					 option

		-	Add this entry to, grunt.initConfig({
				pkg: grunt.file.readJSON('package.json'),

				csslint: {
					strict: {
						options: {

						},
						src: ['www/css/sample.css']
					},
					laxed: {
						options: {
							'zero-units': false // passing external source as options can be done using,
							csslintrc : 'lintrules.json'
						},
						src: ['www/css/sample.css']
					}
				}
			});

			Run laxed in command line as follows, $grnt csslint:laxed

	# Using cssmin:

		-	Add this entry to, grunt.initConfig({
				pkg: grunt.file.readJSON('package.json'),

				cssmin: {
					min: {
						options: {
							"report": "gzip"
						},
						files: {
							'www/css/sample.min.css': ['www/css/sample.css']
						}
					}

					// To use minify multiple files dynamically, use minify property rather than min option under cssmin (remove min property)
					minify: {
						expand: true,
						cwd: './www/css/',
						dest: './www/css/',
						src: ["*.css", "!*.min.css"]', // To not to already minified files, negate the file extension
						ext: '.min.css',
						extDot: 'last'
					},

					// To create one minified css file
					concat: {
						options: {

						},
						files: {
						 	'www/css/sample.min.css': ['www/css/*.css']
						}
					}

				}
			});

# Advanced grunt: Scaffholding and custom plugins

	# grunt init scaffholding: To automate project automation

		$ npm install -g grunt-init

		clone the git repository to local
		$ git clone <url> %USERPROFILE%\.grunt-init\gruntfile

		$ grunt-init gruntfile -> run this to scaffhold

	# Custom inline tasks: Register the task to create a custom task
		-	Use grunt.registerTask -- requires name and description, invoked via callback, can access external variables and 		libraries, can accept arguments in the callnback

		ex:

		var fs = require('fs');

		grunt.registerTask('checkFileSize', 'Task to check the file size', function(debug) {
			var options =  this.options({
					folderToScan: '';
				});

			if(this.args.length !== 0 && debug !== undefined) {
				grunt.log.writeflags(options, 'Options');
			}

			grunt.file.recurse(options.folderToScan, function(abspath, rootdir, subdir, filename) {
				if(grunt.file.isFile(abspath)) {
					var stats = fs.statSync(absPath); -- fs is a node library to work with file system (import required)
					var asBytes = stats.size / 1024;
					grunt.log.writeln("Found file %s with size of %s kb", filename, asBytes);
				}
			});
		});

		-	To make custom tasks work like a normal grunt task
			grunt.initConfig ({
				checkFileSize: {
					options: {
						folderToScan: './files';
					}
				}
			});

		-	Providing arguments to task, add debug parameter to checkFileSize function() and check it's populated.					Arguments can be passed as,

			$ grunt checkFileSize:true

	# Creating Custom plugins:

		-	Clone gruntplugin to local as specified above
		-	run, $ grunt-init gruntplugin
			Asks few questions, answer them, which created folder structure and required files for an plugin

		-	Creates CheckFileSize.js -> add the following code,

			'use strict';

			module.exports = function(grunt) {
					grunt.registerTask('checkFileSize', 'Task to check the file size', function() {
					var options =  this.options({
							folderToScan: '',
							debug: false
						});

					if(debug !== undefined) {
						grunt.log.writeflags(options, 'Options');
					}

					grunt.file.recurse(options.folderToScan, function(abspath, rootdir, subdir, filename) {
						if(grunt.file.isFile(abspath)) {
							var stats = fs.statSync(absPath); -- fs is a node library to work with file system (import required)
							var asBytes = stats.size / 1024;
							grunt.log.writeln("Found file %s with size of %s kb", filename, asBytes);
						}
					});
				});
			}

		-	To use it, update the configuration file, GruntFile.js

			CheckFileSize: {
				options: {
					folderToScan: './files',
					debug: true					}
			},

			// Loads this plugin's tasks
			grunt.loadTasks('tasks');

		- 	Install the package before using it, run, $ npm install

			grunt.fail.fatal(); // method used to write the error messages to IO

		- 	Publishing to npm:

			$ npm adduser - to add the user details
			$ npm publish - to publish the plugin