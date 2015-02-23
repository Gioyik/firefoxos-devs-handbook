# Working on Gecko (aka B2G)

## Getting Gecko

## Building Gecko

## Using Try server
The [try server](https://treeherder.mozilla.org/#/jobs?repo=try) is an easy way to test a patch without actually checking the patch into the core repository. Your code will go through the same tests as a mozilla-central push, and you'll be able to download builds if you wish.

To use try server, you need a [Mozilla hg account](http://www.mozilla.org/hacking/committer/) ([level 1](http://www.mozilla.org/hacking/commit-access-policy/) is sufficient). 

And useful tool is TryChooser, it lets you select specific jobs you want to run on tryserver. You use TryChooser by putting a try: block at the end of the topmost commit message when you push to try. 

### Examples of TryChooser syntax
Run all platforms, all tests, no talos. You probably want to narrow down the test scope though if your change does not affect all platforms and all unit tests.: 
```
try: -b do -p all -u all -t none
```

Debug and optimized builds, all available platforms, only crashtests, no talos: 
```
try: -b do -u crashtest
```

Linux debug build, no unittests, no talos: 
```
try: -b d -p linux -u none
```

Debug and optimized builds for all platforms, all unittests, select talos: 
```
try: -b do -u all -t chrome,nochrome
```

Same as above but run talos in profiling mode so that it produces zip files with profiles that can be opened in Cleopatra. See Talos Profiling. 
```
try: -b do -u all -t chrome,nochrome mozharness: --spsProfile
```

Opt builds on all available platforms, select unittests, select talos 
```
try: -b o -u jsreftest,crashtest,mochitest-1 -t tp4,scroll
```

Opt builds on all available platforms, all unittests, no emails, results posted to bug 456234 
```
try: -b o -u all -n --post-to-bugzilla bug 456234
```

Debug builds for 64-bit OSX, run all tests only on OSX 10.7 machines 
```
try: -b d -p macosx64 -u all[10.7]
```

Debug builds for 64-bit OSX, run all tests but not on all OSX test platforms except OSX 10.7 
```
try: -b d -p macosx64 -u all[-10.7]
```

"T push" -- good replacement for -p all -u all. Builds on all platforms, runs all tests on linux64. 
```
try: -b do -p all -u all[x64]
```

Same as above, but run the tests on OSX 10.8 instead 
```
try: -b do -p all -u all[10.8]
```

#### Syntax Builder

To simplify using the TryChooser, you can use the [TryChooser Syntax Builder](http://trychooser.pub.build.mozilla.org/) website or the [mercurial extension](http://hg.mozilla.org/users/pbiggar_mozilla.com/trychooser/file/tip). 

### Push to TryChooser

#### With Mercurial Queues MQ

Note: If you are running Mercurial 2.1 or higher, see this note about phases. (bug 725362)

If you're using Mercurial Queues, you can use TryChooser by adding an empty patch with a "try:" string to the top of your queue:

```
hg qgoto your-patch
hg qnew -m "try: <Insert options here>" try-your-patch
hg push -f -rtip ssh://hg.mozilla.org/try
```

#### Without Mercurial Queues (MQ)

If you're not using Mercurial Queues, you can use TryChooser by including a "try:" string in the message of the commit you push to try:

```
$ (edit your code)
$ hg commit -m "Bug XXXXXX - Change foo to bar.  try: -b d -p all"
$ hg push -f ssh://hg.mozilla.org/try
```

I>After pushing, you can remove the empty patch:
I>```
I>hg qpop
I>hg qremove try-your-patch
I>```

### See on TryChooser

You can see the results of your tryserver build in a number of ways:

* You'll get an email on a successful push with a link to Treeherder for your revision as well as optional emails on any non-successful build/test/talos results.
* You can have the results of your try run posted to bug(s) automatically at the completion of the run using the --post-to-bugzilla flag in your try syntax.
* Look for your changeset on Treeherder. You can add &author=YOUR.EMAIL to only see your pushes. Example: `https://treeherder.mozilla.org/#/jobs?repo=try&author=example@example.com`