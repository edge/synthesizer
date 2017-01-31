# synthesizer

A lightweight, practical task runner. Great for build systems, testing, and deployments!

## Install

Install `synthesizer` globally:
```sh
$ sudo yarn global add synthesizer
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

## API

### `register(name : string, ...tasks : function | string)`
`register` will add a new task that is identified by `name`. When run, the `register`ed task will sequentially run each of its component tasks. In addition to providing functions, other `register`ed tasks can also be sourced. This is great for composing tasks and building workflows.

```js
register('a', 'x', some_function, 'y', 'z')
```

Running `syn a` will execute `'x'`, `some_function`, `'y'`, and `'z'` in order.

### `run(name : string, ?args : [string], ?options : {})`
`run` is based off of `child_process.spawnSync`, providing some convenient defaults and error handling.

```js
register('shello', () => {
	run('echo', ['hello, world!'])
})
```

```sh
$ syn shello
  {#syn} starting in ~/Documents/projects/use-syn
  {#syn} using ~/Documents/projects/use-syn/syn.js
  {:shello} init
  hello, world!
  {:shello} done
  {#syn} ok
```

### `ask(?prompt : string) : string`
`ask` provides a simple readline interface for input. You can optionally provide a prompt. Useful with `cat`!

```js
register('login', () => {
	const username = ask('user: ')
	const password = ask('pass: ')

	run('echo', [`do something with ${ username }:${ password }`])
})
```