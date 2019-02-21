# Aliyun Function Compute Serverless Plugin
### This forked repo contains customizations & bugfixes, details in changelog
View the original README [here](https://github.com/aliyun/serverless-aliyun-function-compute/blob/master/README.md)

Steps to prepare your repo for this serverless plugin:
1. On repo root, ```mkdir plugins && cd plugins && git submodule add https://github.com/Feedmee-app/serverless-aliyun-function-compute.git```
2. Copy serverless.yml from this ./template into your repo's root, then customize functions. Example [here](./test/project/serverless.yml), you can take [this](./test/project) as a template for your function
3. Make a circleCI config file, template [here](./template/.circleci)
4. Add .serverless to .gitignore
5. Add these to your package.json (drop these versions if you have some of the libraries)
	```json
	  "dependencies": {
		"@alicloud/cloudapi": "^1.1.0",
		"@alicloud/fc": "^1.1.0",
		"@alicloud/log": "^1.0.0",
		"@alicloud/ram": "^1.0.0",
		"ali-oss": "^4.8.0",
		"chalk": "^2.0.1",
		"co": "^4.6.0",
		"fs-extra": "^3.0.1",
		"ini": "^1.3.4",
		"lodash": "^4.17.4"
	  },
	  "devDependencies": {
		"eslint": "^4.6.1",
		"jest": "^20.0.4",
		"sinon": "^2.4.1"
	  }
	```
6. Add the following environment variables to circleCI
	```yaml
	$ACCESS_KEY_ID for access key id
	$ACCESS_KEY_SECRET for key secret
	$ACCOUNT_ID for account id
	```
	You can change the variable names as you wish, but this is the defaults for circleCI to write to config file.
	Future versions of this repo will use the env directly instead of reading from the config file
* Notes:
	- DO NOT run ```serverless plugin install --name serverless-aliyun-function-compute```, as it installs the public version. 

For development locally:
1. ```npm install -g serverless```
2. echo
	```
	[default]
	aliyun_access_key_id = XXXX
	aliyun_access_key_secret = XXXX
	aliyun_account_id = XXXX
	```
	to ~/.aliyuncli/credentials
3. ```serverless deploy --stage production --region ap-southeast-2```
	
	--stage is just a name, think of branches

### Known issues
1. HTTP triggers in config does not actually create the right config (makes API endpoints etc, but does not create the actual trigger), you have to manually create the triggers
2. OSS triggers to work, tho keep in mind that one bucket can only have one trigger, you might need to disable your current trigger before doing this
3. Upload takes time as your node_modules folder size grow, because this plugin actually zips the entire repo and upload to OSS then update the function from OSS
4. Environment variables is not tested yet, just set them manually

### Other
1. ```git pull --recurse-submodules``` to update you local repo with this
