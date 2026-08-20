
#  Adding custom robots.txt via composer
This set of shared custom robots.txt directives are intended to be best-practice for the bulk of Web Services sites. If you need to add site-specific robots.txt rules to a repo, these should be added in the `.platform.sh.yaml` build hook.
##  Add shared rules to repo
###  Update composer.json
####  Add file-mapping
In the `extra.drupal-scaffold` key, add a new file mapping.
```
"locations": {
	"web-root": "web/"
}
```
will become:
```
"locations": {
	"web-root": "web/"
},
"file-mapping": {
	"[web-root]/robots.txt": {
		"append": "assets/robots-custom/robots-custom.txt"
	}
}
```
####  Add new installer-path
In the `extra.installer-paths` key, add the path to store all packages that are assigned the type `robots`.
```
"assets/{$name}": [
	"type:robots"
],
```
####  Add a new installer-type
In the `extra.installer-types` key, we need to add robots to the list.
```
"installer-types": [
	"drupal-library",
	"drupal-recipe"
],
```
will become:
```
"installer-types": [
	"drupal-library",
	"drupal-recipe",
	"robots"
],
```
###  Update the gitignore
The root `.gitignore` should ignore the `assets` directory where the package will be installed, so add:
`/assets/`
###  Add the package via composer
Add our custom robots package with:
`composer require ubc-web-services/robots-custom:dev-master`
###  Commit changes
Commit all changes, including the updated `robots.txt` in the `/web` directory.
##  Add custom site-specific robots.txt rules
In the `.platform.sh.yaml` file, add a build hook for your custom rules. For example:
```
hooks:
	build: |
		set -e
		# Custom robots.txt rules
		cat >> web/robots.txt <<'EOF'
		Disallow: /test-path
		Disallow: /another-test-path
		EOF
```
**Do not** add the custom rules via composer unless you want to avoid using the rules specified in the shared repo. Only one set of rules can be appended to the `robots.txt` file via composer.