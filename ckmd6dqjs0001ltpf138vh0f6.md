## A Quick Guide for Switching to Gulp 4

A new version of gulp is here. What do you **need** to know? Will your `gulpfile`s break?

There are various changes listed in the [CHANGELOG](https://github.com/gulpjs/gulp/blob/4.0/CHANGELOG.md) for `gulp@4.0`. The majority are non-breaking. The major breaking change is the introduction of `gulp.series` and `gulp.parallel`.

Before going any further, thanks to Gulp for sharing this guide over on Twitter 🐦!

> [Check out this tweet!](https://twitter.com/gulpjs/status/957412715974164480?ref_src=twsrc%5Etfw)

For those in camp **TL;DR** — not a huge impact. Your tasks will break if they use the list parameter to define task dependencies. It’s not a huge refactor though to get things back in working order. You can see a gist using `gulp@4.0` below and here’s a [link](https://github.com/jh3y/gulp-boilerplate) to a `gulp` boilerplate using `gulp@4.0`.

    import gulp from 'gulp'
    
    const compileMarkup = () => { // COMPILE MARKUP }
    const compileScript = () => { // COMPILE SCRIPT }
    const compileStyle = () => { // COMPILE STYLE }
    
    const watchMarkup = () => { // WATCH MARKUP }
    const watchScript = () => { // WATCH SCRIPT }
    const watchStyle = () => { // WATCH STYLE }
    
    const compile = gulp.parallel(compileMarkup, compileScript, compileStyle)
    compile.description = 'compile all sources'
    
    // Not exposed to CLI
    const startServer = () => { // START SERVER }
    
    const serve = gulp.series(compile, startServer)
    serve.description = 'serve compiled source on local server at port 3000'
    
    const watch = gulp.parallel(watchMarkup, watchScript, watchStyle)
    watch.description = 'watch for changes to all source'
    
    const defaultTasks = gulp.parallel(serve, watch)
    
    export {
      compile,
      compileMarkup,
      compileScript,
      compileStyle,
      serve,
      watch,
      watchMarkup,
      watchScript,
      watchStyle,
    }
    
    export default defaultTasks

Series + Paralell
-----------------

This is the big change. It seems there has been a big effort made towards enabling more control over how tasks run 🛂

### Always in parallel or chained ⛓

In previous versions of `gulp` we could pass a list parameter to a task. This would define a set of tasks that would run before that task.

    gulp.task('a', ['b', 'c'], function () { // do something})

In this example, task `a` relies on tasks `b` and `c` to run before it will. Tasks `b` and `c` will run in parallel. You don’t have control over their running order unless you create a chain;

    gulp.task('a', ['c'], function () { // do something})
    gulp.task('c', ['b'], function () { // do something})

This ensures a running order of `b`, then `c`, then `a`. If you need to chain tasks, it could soon get confusing. Task metadata and good docs might help but wouldn’t it be much easier to have something like;

    gulp.task('a', series(['b', 'c']), function () { // do something})

That’s pretty much what you’re getting with `gulp@4.0` 🎉

### The new kids on the block

    gulp.series()
    gulp.parallel()

Rather intuitive. The list parameter gets deprecated 👢

If we look at our first example;

    gulp.task('a', ['b', 'c'], function () { // do something })

To convert this for `gulp@4.0`

    gulp.task('a', gulp.series(gulp.parallel('b', 'c'), function () {
      // do something
    }))

If you’re switching to `gulp@4.0`, and you haven’t already, you’ll likely want to stop using anonymous functions. They print `<anonymous>` when run and aren’t very useful when viewing task metadata. If you haven’t used task metadata, I recommend it. You can read about it [here](https://medium.com/gulpjs/gulp-sips-custom-task-metadata-9a2dc80ac7b1).

You could change the above to

    gulp.task('a', gulp.series(gulp.parallel('b', 'c'), function a () {
      // do something
    }))

But to make it even better 👍

    var a = function () { // do some stuff }
    gulp.task('a', gulp.series(gulp.parallel(b, c), a))

Or better than that

    var a = gulp.series(gulp.parallel(b, c), a)
    module.exports = { a:a }

Yep that does actually work. You can `export` functions as tasks. This isn’t specific to `gulp@4.0` but if you’re switching why not try out something new? You can see more on `exports` as tasks [here](https://github.com/gulpjs/gulp/blob/master/docs/recipes/exports-as-tasks.md).

I’d switch out the variable `a` declaration there though. Though it would still work via hoisting.

    var doStuff = gulp.series(gulp.parallel(b, c), a)
    module.exports = { doStuff: doStuff }

Let’s take it a little further. Odds are you are using newer `JavaScript` syntax via `babel` etc.

    const doStuff = gulp.series(gulp.parallel(b, c), a)
    export { doStuff }

Or

    export const doStuff = gulp.series(gulp.parallel(b, c), a)

That’s tidy 😎

If you don’t mind, the rest of the examples will use the newer syntax style 👍

Right, back to `gulp@4.0`. The new API encourages you to think more about the actual structure of your build tasks. It also advocates smaller comprehensive tasks which provides better flexibility when making changes.

### A real example

Let’s consider a real use case.

A common default task is something like `develop`. This may have once looked like

    gulp.task('develop', ['serve', 'watch'])

It should be pretty self explanatory what these tasks do;

*   `serve` — serves up assets on a local static server
*   `watch` — watches all source and triggers compilation on change

This makes perfect sense to run in `parallel`

    const defaultTasks = gulp.parallel(serve, watch)
    export default defaultTasks

An example of `series` in use? It’s likely that `serve` task will want to compile assets before serving. Something like

    const compileAssets = gulp.parallel([compileScript, compileStyle])
    const startServer = () => { // magic }
    const serve = gulp.series(compileAssets, startServer)
    export { serve }

And that’s all there is to it!
------------------------------

Migration to `gulp@4.0` should be pretty straightforward. The documentation is good and there are plenty of resources out there to help you if you get stuck. I’ve left some links below and an example `gist` using the new syntax. You can also check out a `gulp` boilerplate using `gulp@4.0` [here](https://github.com/jh3y/gulp-boilerplate) 👍🤓

As always, any questions or suggestions, please feel free to leave a response or [tweet](https://twitter.com/@jh3yy) me 🐦!

### Some links

*   [Exports as tasks](https://github.com/gulpjs/gulp/blob/master/docs/recipes/exports-as-tasks.md) — Gulp on Github
*   [Custom task metadata ](https://medium.com/gulpjs/gulp-sips-custom-task-metadata-9a2dc80ac7b1)— Gulp on Medium

    import gulp from 'gulp'
    
    const compileMarkup = () => { // COMPILE MARKUP }
    const compileScript = () => { // COMPILE SCRIPT }
    const compileStyle = () => { // COMPILE STYLE }
    
    const watchMarkup = () => { // WATCH MARKUP }
    const watchScript = () => { // WATCH SCRIPT }
    const watchStyle = () => { // WATCH STYLE }
    
    const compile = gulp.parallel(compileMarkup, compileScript, compileStyle)
    compile.description = 'compile all sources'
    
    // Not exposed to CLI
    const startServer = () => { // START SERVER }
    
    const serve = gulp.series(compile, startServer)
    serve.description = 'serve compiled source on local server at port 3000'
    
    const watch = gulp.parallel(watchMarkup, watchScript, watchStyle)
    watch.description = 'watch for changes to all source'
    
    const defaultTasks = gulp.parallel(serve, watch)
    
    export {
      compile,
      compileMarkup,
      compileScript,
      compileStyle,
      serve,
      watch,
      watchMarkup,
      watchScript,
      watchStyle,
    }
    
    export default defaultTasks