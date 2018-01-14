
<table width='100%'>
    <tr>
        <td align='left' width='100%' colspan='2'>
            <strong>Lerna Tutorial</strong><br />
            Learn how to create and mantain monorepos with lerna.
        </td>
    </tr>
    <tr>
        <td>
            A FOSS (Free & Open Source Software) project. Maintained by <a href='https://github.com/ahmadawais'>@AhmadAwais</a>.
        </td>
        <td align='center'>
            <a href='https://AhmadAwais.com/'>
                <img src='https://i.imgur.com/Asg4d3k.png' width='100' />
            </a>
        </td>
    </tr>
</table>

# Lerna tutorial

[![Greenkeeper badge](https://badges.greenkeeper.io/ahmadawais/lerna-tutorial.svg)](https://greenkeeper.io/)

First off, What is [`lerna`](https://github.com/lerna/lerna)? [`lerna`](https://github.com/lerna/lerna) is a tool that allows you to maintain multiple `npm` packages within one repository.

There's a couple of benefits to this kind of approach, the paradigm is called a `monorepo`, and more can be read about it from the [source of `babel`, and `react`](https://github.com/babel/babel/blob/master/doc/design/monorepo.md).

Here's the gist:

* Single lint, build, test and release process.
* Easy to coordinate changes across modules.
* Single place to report issues.
* Easier to setup a development environment.
* Tests across modules are ran together which finds bugs that touch multiple modules easier.

## Getting started.

For this demo I'm going to install [`lerna`](https://github.com/lerna/lerna) is a CLI (command line interface) tool. You're going to want to install it with the `--global` (`-g`) flag.

```
npm i lerna -g
```

Then once it's done installing your going to want to run the following

```
lerna init
```

This will do a couple of things.

* Creating packages folder.
* Updating package.json.
* Creating lerna.json.

The `/packages` folder is where all of your packages belong. Let's go about making a new package `aa-alpha`.

```
cd packages
mkdir aa-alpha
cd aa-alpha
npm init -y
echo "module.exports = 'aa-alpha'" > index.js
```

Lets go through the same steps for another package `aa-beta`.

First go up one directory:

```
cd ..
```

Now go about creating `aa-beta`.

```
mkdir aa-beta
cd aa-beta
npm init -y
echo "module.exports = 'aa-beta'" > index.js
```

Now we're going to create a `usage` package that uses both `aa-alpha` and `aa-beta` as dependencies.

First go up one directory:

```
cd ..
```

Now go about creating `\usage`.

```
mkdir usage
cd usage
npm init -y
touch index.js
```

Open up `/packages/usage/index.js` in a text editor and paste this in.

```js
var aa-alpha = require('aa-alpha')
var aa-beta = require('aa-beta')
console.log(aa-alpha + " " + aa-beta)
```

We're almost there. At this point your whole project should look something like this:

```
.
├── README.md
├── lerna.json
├── package.json
└── packages
    ├── aa-alpha
    │   ├── index.js
    │   └── package.json
    ├── aa-beta
    │   ├── index.js
    │   └── package.json
    └── usage
        ├── index.js
        └── package.json
```

What you want to do now is go into `/packages/usage/package.json` and add these lines under `dependencies`.

```json
{
  "dependencies": {
    "aa-alpha": "1.0.0",
    "aa-beta": "1.0.0"
  }
}
```

Now you need to wire everything up with this command.

```
lerna bootstrap
```

The output from this command should look something like this:

```
Lerna v2.0.0-aa-beta.20
Linking all dependencies
Successfully bootstrapped 3 packages.
```

Now using the `tree` command once more (`brew install tree`) we can see the folder structure we can see what [`lerna`](https://github.com/lerna/lerna) did.

```
.
├── README.md
├── lerna.json
├── package.json
└── packages
    ├── aa-alpha
    │   ├── index.js
    │   ├── node_modules
    │   └── package.json
    ├── aa-beta
    │   ├── index.js
    │   ├── node_modules
    │   └── package.json
    └── usage
        ├── index.js
        ├── node_modules
        │   ├── aa-alpha
        │   │   ├── index.js
        │   │   └── package.json
        │   └── aa-beta
        │       ├── index.js
        │       └── package.json
        └── package.json
```

It added two `stubbed` (my term not [`lerna`](https://github.com/lerna/lerna)'s) modules. If you peak inside `/packages/usage/node_modules/aa-alpha/index.js` you can see what I mean.

> contents of `./packages/usage/node_modules/aa-alpha/index.js`

```js
module.exports = require("/Users/user/Desktop/lerna-tutorial/packages/aa-alpha");
```

> Note: This is an absolute path to the module. So if you ever move your lerna project you'll need to rerun `lerna bootstrap`.

And volia! When we run `node ./packages/usage/index.js` we get our expected output!

```
aa-alpha aa-beta
```

### 🙌 [WPCOUPLE PARTNERS](https://WPCouple.com/partners):
This open source project is maintained by the help of awesome businesses listed below. What? [Read more about it →](https://WPCouple.com/partners)

<table width='100%'>
	<tr>
		<td width='333.33'><a target='_blank' href='https://www.gravityforms.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtrE/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://kinsta.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mu5O/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://wpengine.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mto3/c' /></a></td>
	</tr>
	<tr>
		<td width='333.33'><a target='_blank' href='https://www.sitelock.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtyZ/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://wp-rocket.me/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtrv/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://blogvault.net/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtph/c' /></a></td>
	</tr>
	<tr>
		<td width='333.33'><a target='_blank' href='http://cridio.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtmy/c' /></a></td>
		<td width='333.33'><a target='_blank' href='http://wecobble.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtrW/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://www.cloudways.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mu0C/c' /></a></td>
	</tr>
	<tr>
		<td width='333.33'><a target='_blank' href='https://www.cozmoslabs.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mu9W/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://wpgeodirectory.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtwv/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://www.wpsecurityauditlog.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtkh/c' /></a></td>
	</tr>
	<tr>
		<td width='333.33'><a target='_blank' href='https://mythemeshop.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/n3ug/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://www.liquidweb.com/?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mtnt/c' /></a></td>
		<td width='333.33'><a target='_blank' href='https://WPCouple.com/contact?utm_source=WPCouple&utm_medium=Partner'><img src='http://on.ahmda.ws/mu3F/c' /></a></td>
	</tr>
</table>
