# mod-less is LESS apache2 module :

This apache2 module is written to compile LESS code inside the css files when they are requested so the process will 
be transparent from any script using css no matter what it's language is

this apache2 module is written using c.

# How it works, Default configuration :

when a request is sent to any css file this request will be handeled by this module. The module will look for a less file of the same name and compare the timestamps of those files. If the less file has been changed since the last request, the modules calls lessc and compiles a new version of the the css file. If the css file is recent, no recompile will take place. In both cases, the compiled css file will be sent back to the client.

example:
1. request: http://localhost/themes/css/style.css
2. the module will look for a http://localhost/themes/css/style.less and compare timestamps
  1. it will then eventually recompile http://localhost/themes/css/style.css from it
3. content of the compiled http://localhost/themes/css/style.css is delivered

*Advantages:* When deploying your app from the test-system to the live-system (where you wouldn't use mod_less, would you?), the compiled css-files will be deployed, too, and all links will stay intact
*Disadvantages:* The modules gets invoked on every css file, if it has a corresponding less-file or not. It whould not impact performance greatly, but who knows...

# How it works, Alternative configuration :

this configuration needs to be enabled in less.conf

when a request is sent to any css file this request will just be passed to apache and *not* be handled by this module. No automatic recompilation will be triggered when accessing css files.
To enable automatic recompilation, instead point your application to the less file. Requests on less files are handled by this module and, as above, a recompilation will eventually be triggered. In any case, the compiled css file will be sent back to the client.

example:
1. request: http://localhost/themes/css/style.less
2. the module will look for a http://localhost/themes/css/style.css and compare timestamps
  1. it will then eventually recompile http://localhost/themes/css/style.css from it
3. content of the compiled http://localhost/themes/css/style.css is delivered

*Advantages:* No extra code running when accessing css files
*Disadvantages:* You need to change your application to point to the less files for automatic recompilationd (and back after deployment)

# Dependencies : 

	Apache2
		sudo apt-get install apache2

	Less Css 
		
		sudo apt-get install rubygems1.8
		sudo gem install rubygems-update
		sudo update_rubygems     
		sudo gem install less

# Installation :
	
	copy /bin/mod_less.so to /usr/lib/apache2/modules/mod_less.so
		  chmod 644 /usr/lib/apache2/modules/mod_less.so
	
	copy /bin/less.load to /etc/apache2/mods-available/less.load
	copy /bin/less.conf to /etc/apache2/mods-available/less.conf

you can edit the less.conf file to change when this module is gonna be invoked 

	for example : this conf with "\.css$" will invoke the module when any .css file is requested 
		<FilesMatch "\.css$">
		   SetHandler less
		</FilesMatch>

	changing "\.css$" to "\.less$" will invoke the module only for requested files with .less extension

# Compile from source :
source can be found in /src/mod_less.c
to compile it you will need the apxs2 tool which can be found in the apache2-prefork-dev package

	sudo apt-get install apache2-prefork-dev

after installing apxs2 tool, cd to the src directory and execute this command 

	sudo apxs2 -c -i mod_less.c

compiled file "mod_less.so" will be automatically copied to /usr/lib/apache2/modules/mod_less.so and chmoded.
you only need to add the less.load and less.conf as mentioned in the Installation section above.

# Developers :

waleed al qadi (waleedq).
	
	note: feel free to fork and contrib to this project
		
	
