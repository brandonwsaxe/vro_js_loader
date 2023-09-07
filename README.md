# vro_js_loader


/* vro browserify module loader */

code.vmware.pve.contrib.js
code.vmware.pve.contrib.js.module()
code.vmware.pve.contrib.js.script()
code.vmware.pve.contrib.js.bundle()


steps.
1. install node (to get npm)
2. install browserify per https://browserify.org/#install
e.g. npm install -g browserify
3. find the npm package you want from https://www.npmjs.com/
4. install npm package. e.g. `npm install uri-js`
5. create a javascript file that will 'require' the npm package. to export the required module, wrap the require call inside a 'bundle' function call. the vro action provides the 'bundle' function.
e.g echo "bundle(require('uri-js'))" > 'require_urijs.js'
6. run browserify against the js file. browserify will see the 'require' call and generate a static js bundle that can be copy/pasted into a vRO action.
e.g. browserify require_urijs.js -o vro_uri.js
7. open the output file 'vro_uri.js' and copy its contents
8. create a new action and paste its contents. Then save the action.
e.g. 'bundles.urijs'
9. Create an action or vro workflow to use the module
a. first load the 'bundle' action.
e.g. const bundle = System.getModule("code.vmware.pve.contrib.js").bundle()
b. second load the module using the same syntax as you'd normally call the action, but inside quotes. this ensures that vRO will see the bundle as a dependency when you build packages.
	URI = bundle('System.getModule("bundles").urijs()');
c. use the module
e.g. URI.parse(<params>)
