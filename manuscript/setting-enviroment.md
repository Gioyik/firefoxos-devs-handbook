## Setting Up Your Environment

1. Forking and cloning gaia
    - Relevant repos:
        - https://github.com/mozilla-b2g/B2G
        - https://github.com/mozilla-b2g/gaia
    - Add the upstreams:
        - `git remote add upstream https://github.com/mozilla-b2g/B2G`
        - `git remote add upstream https://github.com/mozilla-b2g/gaia`
    - In gaia, make remotes for bocoup team members
        - Rick has a [script](https://github.com/bocoup/gaia-notes/blob/master/gaia_remotes.sh) to provide shortcuts, but as repo names are case sensitive your needs (e.g. wanting all lowercase while working) may not fit the script as such.
2. Download and install Firefox Nightly
    - http://nightly.mozilla.org/
    - Copy your absolute path to the Firefox Nightly application for step 3 below
    - If you need to go back to a known good build for nightly:
        - Go to the nightly [FTP trunk](https://ftp.mozilla.org/pub/mozilla.org/firefox/nightly/)
        - Look for folders ending in "mozilla-central"
        - eg. https://ftp.mozilla.org/pub/mozilla.org/firefox/nightly/2013-07-19-03-02-04-mozilla-central/
3. Get Android Developer Tools
    - Windows or OSX
        - http://developer.android.com/sdk/index.html
        - On OS X you can download the sdk and add the `platform-tools` folders to your `$PATH` in order to get [adb](http://developer.android.com/tools/help/adb.html). 
    - Linux
        - android-tools-* in your packages repo
4. Download and install the closure linter `gjslint`
    - https://developers.google.com/closure/utilities/docs/linter_howto
5. Install the node dependencies for the test infrastructure
    - cd /path/to/gaia/tools/test-agent
    - npm install
