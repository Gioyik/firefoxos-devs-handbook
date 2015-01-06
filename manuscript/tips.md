# Tips
Those tips could save your life someday.

## png_recompress.sh
This is an image compressor. You can run it on your gaia directory with the following command structure:

    $ ./tools/png_recompress.sh -v bg.png

This will remove unnecessary metadata and further compress the png; it can save up to 99.5% of the file size, which has a very measurable impact on an app's ZIP size and memory footprint.

## Hardware keys emulation

    +-------------------+-------------------------+---------------+
    | Button            | Window/Linux            | Mac           |
    +-------------------+-------------------------+---------------+
    | Home              | Home key                | fn + ←        |
    | Power             | End                     | fn + →        |
    | Volume            | Page Up/Page Down       | fn + ↑/↓      |
    | Open Cards View   | Long press to Home Key  | cmd + fn + ←  |
    +-------------------+-------------------------+---------------+

## `b2g` vs. `b2g-bin`

The `b2g` executable uses the default profile that ships with Boot 2 Gecko
itself. This makes it unsuitable for Gaia development. The `b2g-bin` executable
uses the profile specified on the command line (see above).

## Grab the latest from master

```bash
cd path/to/gaia
git checkout master
git pull upstream master
```

#### Make a change and test locally

1. Check out a new branch

        git checkout -b busted

2. Open editor, navigate to apps/clock/js
3. Open clock_view.js
4. Break something in the file, for example: comment out a big chunk of code
5. Make a debug profile

        DESKTOP_SHIMS=1 NOFTU=1 DEBUG=1 make
        
6. Launch Firefox Nightly with the newly created debug profile
        
        [/path/to]/firefox -profile [/path/to]/gaia/profile-debug http://test-agent.gaiamobile.org:8080

7. This will open FireFox Nightly, displaying the "test-agent"
    - if you want to see a specific app (e.g. clock), type `clock.gaiamobile.org:8080` into the url bar
8. Open the Console, uncheck the following noisy as hell items
    - Network
    - CSS
    - Security

#### To run the unit tests for an app from the command line (using Corey's functions from b2g.sh)

1. Start the test agent server
    - cd /path/to/gaia
    - make test-agent-server

2. Start an instance of gaia in firefox nightly 
    - You need to have a debug profile in your firefox for this to run. So if you haven't made one
        -  cd /path/to/gaia
        -  DESKTOP_SHIMS=1 NOFTU=1 DEBUG=1 make
    - Run the gaia-test-latest function from b2g.sh. You probably want to background this to get your console back

3. Run the unit test
    - cd /path/to/gaia
    - gaia-unit APPNAME

The tests will then run. At this point for any failing tests the test server will be watching that file for changes and re-running that test on save.
If you add a new test file you need to restart the test-server.


### General Notes About Branches and Bugs

Branch names should only be the bug number (ease of remembering and ease of pulling through CLI).  This also helps enforce mapping bugs to PRs.

e.g. `git pull rwaldron 12345`

or `git checkout -b 88712 rwaldron/88712` to create a new branch based off that person's current branch

### Rebasing

Master branch changes constantly (many, many times a day). A patch which could take 2 hours might find master branch changes underneath you.

From working branch (e.g. `busted`):

1. First attempt to rebase

```bash
git checkout -b busted-r1
git pull --rebase upstream master
```

2a. If no conflicts -->

```bash
git checkout busted
git pull --rebase upstream master
git branch -D busted-r1
```

2b. If conflicts, work them out with the other developer and repeat above

### Making a Pull Request

##################
After fixing a bug
##################

### Try to rebase with upstream
- git checkout -b #####-r1
- git pull --rebase upstream master
- git checkout #####

### If rebase doesn't work, from #####-r#
- Resolve the conflict
- git add -i the file
- git rebase --continue
- git branch -D #####
- git checkout -b #####

### Stage the relevant files
- git add -i
- stage all the relevant files

### Commit and push the relevant files
- git commit -m "[copy of bug title]"
- git push

### Create pull request
- Go to: https://github.com/evhan55/gaia/compare/#####
- Click "Click to create a pull request for this comparison"
- Summarize everything you did
- Pull request will be created here: https://github.com/mozilla-b2g/gaia/pull/@@@@@

### Submit for review
- Go to: original bug at https://bugzilla.mozilla.org/show_bug.cgi?id=#####
- Click 'Add an attachment'
- Paste the github pull request url into first field
- Paste the github pull request url into second field
- Paste the summary from above into the 'comment' field
- Add 'review: ?' flag
- Choose the person assigned to review it
- Click "Submit"

### Wait for reviews

### Make changes to a patch and push again
- git commit -m "[copy of bug title]" --amend
- git push origin ##### -f
