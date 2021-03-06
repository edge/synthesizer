# synthesizer

A lightweight, practical task runner. Great for build systems, testing, and deployments!

`synthesizer` has the speed of a Makefile and the power of Gulp.

## Install

Install `synthesizer` globally:
```sh
# yarn global add synthesizer
```

Add `synthesizer` to your project:
```sh
$ yarn add synthesizer
```

## Getting Started

After `synthesizer` has been installed globally and in your project, add a synfile (`syn.js`) to your project:  
**`syn.js`**
```js
const { register } = require('synthesizer')

register('hello', () => {
	console.log('hello, world!')
})
```

Now you can run the `hello` task by typing: (note: try using autocomplete!)
```sh
$ syn hello
  {#syn} starting in ~/Documents/projects/use-syn
  {#syn} using ~/Documents/projects/use-syn/syn.js
  {:hello} init
  hello, world!
  {:hello} done
  {#syn} ok
```


You can also run multiple tasks in a single command. Not only is this good for batching work, it is a useful way to configure your tasks!

```js
let env = 'prod'

register('dev', () => {
	env = 'dev'
})

register('start', () => {
	console.log('running in', env, 'mode')
})
```

```sh
$ syn start
  ...
  running in prod mode
  ...
$ syn dev start
  ...
  running in dev mode
  ...
```

## API

### `register(name : string, ...tasks : function | string)`
`register` will add a new task that is identified by `name`. When run, the `register`ed task will sequentially run each of its component tasks. In addition to providing functions, other `register`ed tasks can also be sourced. This is great for composing tasks and building workflows.

```js
register('a', 'x', some_function, 'y', 'z')
```

Running `syn a` will execute `'x'`, `some_function`, `'y'`, and `'z'` in order.

### `run(name : string, ?args : [string], ?options : {})`
`run` is based off of [`child_process.spawnSync`](https://nodejs.org/api/child_process.html#child_process_child_process_spawnsync_command_args_options), providing some convenient defaults and error handling.

```js
register('shello', () => {
	run('echo', ['hello, world!'])
})
```

```sh
$ syn shello
  ...
  hello, world!
  ...
```

### `shell(command : string, ?options : {})`
`shell` is based off of [`child_process.execSync`](https://nodejs.org/api/child_process.html#child_process_child_process_execsync_command_options). Use this instead of `run` if you have a command string and want to spawn it in a subshell.

```js
register('shello', () => {
	shell('echo \'hello, world!\'')
})
```

```sh
$ syn shello
  ...
  hello, world!
  ...
```


### `ask(?prompt : string, ?options : {}) : string`
`ask` provides a simple readline interface for input. You can optionally provide a prompt, and [options](https://github.com/anseki/readline-sync#basic_options) to `readline-sync.prompt`.

```js
register('login', () => {
	const username = ask('user: ')
	const password = ask('pass: ', { hideEchoBack: true, mask: '' })

	run('echo', [`do something with ${ username }:${ password }`])
})
```

```sh
$ syn login
  ...
  user: your_manager
  pass: 
  do something with your_manager:Password1
  ...
```

