Steb-by-Step quick guide to create a webpage on github and your local machine

0. First install [ruby](https://rubyinstaller.org/)
1. Install Jekyll on the local machine
    *a. Open command prompt (or terminal) and run the following commands:
    	'''
        gem install bundler jekyll
        jekyll new my-awesome-site
        cd my-awesome-site
        bundle exec jekyll serve
	'''
    *b. # => Now browse [link](http://localhost:4000)

If you have a special theme from github e.g. minimal-mistakes theme
1. Copy the [clone link](https://github.com/mmistakes/minimal-mistakes)
2. Git clone the provided link on your local machine.
3. You can Remove the [Unnecessary files and folders](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#remove-the-unnecessary):
     .editorconfig
     .gitattributes
     .github
     /docs
     /test
     CHANGELOG.md
     minimal-mistakes-jekyll.gemspec
     README.md
     screenshot-layouts.png
     screenshot.png
4. [Gem based method](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#gem-based-method): 
	a. Update Gemfile
		- add (gem "minimal-mistakes-jekyll") to gemfile
		- run (bundle) in command
		- in -config.yml update (theme: minimal-mistakes-jekyll)
		- run bundle update
	a. 
5. [Remote theme method](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/#remote-theme-method)
6. If you had dependencies issue, [install dependencies](https://mmistakes.github.io/minimal-mistakes/docs/installation/#install-dependencies)
7. On command prompt, go to your web folder
8. run the following commands
    - bundle install
    - bundle exec jekyll build
9. to run your webpage, run
    - bundle exec jekyll serve
10. In your brwoser insert the [link](http://127.0.0.1:4000)
11. After each changes in _config.yml_
