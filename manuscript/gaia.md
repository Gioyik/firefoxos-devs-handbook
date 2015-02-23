# Working on Gaia

## Getting the source

## Modifing the code

## Running Gaia
You can run Gaia in a simulator via WebIDE, directly inside Desktop Firefox, as a dedicated desktop build (Desktop B2G), in an emulator, or on a compatible mobile device. This article provides a summary of how to do each, with links to further information.

### WebIDE with a Firefox OS Simulator
The quickest way to try Gaia is via WebIDE, a developer tool available in Firefox for Desktop. It provides a number of useful tools to help you test, deploy and debug HTML5 web apps on Firefox OS phones and the Firefox OS Simulator, directly from your browser.

In desktop Firefox Browser 34+, open WebIDE using Tools > Web Developer > WebIDE. Open the Runtime menu on the top right to install and start a Simulator.

To run your own Gaia build, the process is a bit more complex, but still pretty simple:

1. Before trying to run this, you should build yourself a Gaia device debug profile — go into your Gaia repo clone, and run DEVICE_DEBUG=1 make. This means that you will be able to debug the internal Gaia apps out of the box, plus you don't have to accept the remote debugging confirmation dialog in the simulator each time you start it up.
2. Open Firefox (Nightly is recommended.)
3. Open WebIDE using Tools > Web Developer > WebIDE.
4. Install a Firefox OS Simulator by going to Select Runtime > Install Simulator in WebIDE and choosing an option. Make sure you use the same simulator version as the Gaia version you are running (so for example, if your Gaia branch is 2.1, you need to use Firefox OS Simulator 2.1.)
5. Go back to Firefox Desktop and select Tools > Add-ons. Click on the Firefox OS Simulator 2.2 Preferences button (or whichever version you want to run your own Gaia build in.)
6. In the preferences, click Select a custom Gaia directory, then choose your Gaia device debug profile directory.
7. Open this simulator in WebIDE, and it should now open with your Gaia profile running.

I>If you want to reset your simulator so it is no longer running a custom Gaia profile, you'll need to go to Firefox **about:config**, find the *extensions.fxos_2_2_simulator@mozilla.org.gaiaProfile pref* (2_2 might be different depending on what simulator version you customized), double-click it, delete the value in the resulting dialog box, and press OK.

I>Because Gaia's master branch changes so rapidly, sometimes the latest published simulator falls behind somewhat. If you are trying to run master branch and finding that it won't work (in this case WebIDE will usually give an "Operation timed out" error message), you should download the latest nightly simulator branch and trying it with that instead — visit [Nightly Mozilla Central](http://ftp.mozilla.org/pub/mozilla.org/b2g/nightly/latest-mozilla-central/) and download the fxos-simulator-*.xpi file appropriate for your system. Bear in mind that these nightly builds do not auto-update to future releases. You'd need to switch back to the official builds if you want to get updates.

### B2G Desktop

B2G Desktop is a desktop build of the app runtime used on Firefox OS devices which you can use to run Gaia on your desktop computer. This will soon be replaced by Firefox Mulet (see below), once Mulet is stable enough.

You can download a [nightly build of B2G desktop](http://nightly.mozilla.org/#Desktop%20Boot2Gecko) from the [Firefox Nightly site](http://nightly.mozilla.org/). Depending on what version you are targeting, you may want a specific version of latest-mozilla-b2g18. There are builds for Linux (32 bit and 64 bit), OS X and Windows.

Nightly builds come packaged with a recent version of gaia. Once you've downloaded the archive, all you need to do is extract it to a folder and run the b2g binary from the extracted folder.

    $ cd b2g
    $ ./b2g

To run B2G with your own version of Gaia for development purposes you first need to build a profile from your clone:

    $ cd /path/to/gaia
    $ DEBUG=1 DESKTOP=0 make

This will generate a directory in your gaia directory called profile. The DEBUG part runs Gaia as hosted apps on a built-in web server, rather than the default packaged apps which have to be re-packaged after every change. You can find the path to the profile directory by taking a look at last line of output after running the above command, which should look like:

    Profile Ready: please run [b2g|firefox] -profile /path/to/gaia/profile

You can then run B2G Desktop with your generated profile like so:

    $ ./b2g /path/to/gaia/profile

If you want to, you can build your own B2G desktop from source.

I>On OS X, the b2g binary will be inside B2G.app. To run B2G Desktop on this platform you will need the following command:
<I>    $ ./B2G.app/Contents/MacOS/b2g /path/to/gaia/profile

### Firefox Mulet
It's also possible to run Gaia inside of a special build of Firefox called Firefox Mulet. This gives you the advantages of having a rapid development cycle, as well as standard web development tools and debuggers available to work with.

I>Firefox Mulet is currently at an early stage of development, and you will probably find bugs. Please report them as you come across them.

To run Firefox Mulet with your own version of Gaia for development purposes you first need to build a profile from your clone:

    $ cd /path/to/gaia
    $ DEBUG=1 DESKTOP=0 make

Firefox OS is available for different devices. So if you are going to build for TV or Tablet see the instructions above.

#### TV layout (Stingray)
The Stingray project is an initiative to enable Firefox OS platform to run on larger screen TV 

To build:
    make GAIA_DEVICE_TYPE=tv

#### Tablet layout
The Stingray project is an initiative to enable Firefox OS platform to run on larger screen TV 

To build:
    make GAIA_DEVICE_TYPE=tv

Now that we have a Gaia build ready to run, let's continue preparing Firefox Mulet for the development process:

1. First of all, you need to need to have the Gaia repo cloned on your machine (see Running your own Gaia build for the best way to do this if you want to contribute to the project.) Mulet only works on Gaia 2.2 and above, so it is a good idea to use the master branch.
2. Next, cd into the Gaia repo and build your own profile (see make options reference for the different available types) with make. In the future, Mulet will support multiple build types, and have tools added to allow you to more easily debug apps (for example restarting individual apps if you want to test updates.)
3. Now you need to download a nightly Firefox Mulet build — find this at Mozilla Central. The Mulet builds are the packages whose names start with firefox-*, for example firefox-36.0a1.en-US.mac64.dmg — choose the appropriate build for your development machine.
4. Once downloaded, install the Mulet build somewhere safe so that it doesn't overwrite your normal Firefox Nightly build. For example, on Mac OS X, create a new folder inside Applications called "mulet", and drag it into there.
5. Now run the Mulet build from the command line, passing it your Gaia profile as the profile to use when opening (signified by the -profile option.) For example, you could run something like this from inside your Applications folder on Mac OS X : 

    ./mulet/FirefoxNightly.app/Contents/MacOS/firefox-bin -profile /Users/my-home-folder/git/gaia/profile/

The resulting Mulet build should open up, like so:

[Firefox Mulet](images/firefox-mulet.png)

On this display you've got the standard Firefox Toolbox available for debugging your Gaia apps, plus an emulation of Firefox OS running on the left hand side, and a number of other tools useful to the Gaia context. The contols above the emulator allow you to:

* Choose different screen sizes for the emulator (this is basically Responsive Design View).
* Rotate the emulator screen.
* Turn touch event simulation on and off (if turned off, you can't use the mouse to drag the UI and open apps.)
* Take screenshots.

I>The Home button currently doesn't work, and the display occasionally glitches. You can get round these problems by refreshing the browser tab.

### Real device

W>If you are attempting to Flash Gaia onto a low-memory device such as a Tarako or Spice Fire One, you should Flash it with a special low-memory optimized branch of Gaia, such as the **1.3t branch**. Trying to Flash Gaia's master branch onto a low-memory device will probably result in the phone becoming unresponsive.

To Flash a new version of Gaia onto a real device:

1. First make sure you have the Gaia repo cloned onto your computer, and ADB installed.
2. Make sure you have Debugging via USB enabled.
3. Connect your device to your computer via USB.
4. If there’s a device attached, you can process the following commands. Or you (Windows or Linux distributions user) may need to check OEM USB Drivers page to correctly setup the USB driver on your computer.
5. Run the following command inside the Gaia repo to reset your phone and update your Gaia source code:
    $ make reset-gaia
6. To test non-system apps, you could install them without rebooting the device with the following command:
    $ make install-gaia
7. If you want to install a specific app only, you can pass it through the APP variable, as follows:
    $ make install-gaia APP=browser

I>Pushing Gaia to your device using *make install-gaia / make reset-gaia* builds Gaia with 1x resolution assets by default. To specify higher resolution assets you need to use the **GAIA_DEV_PIXELS_PER_PX** or **GAIA_DPPX** make options (see High resolution image assets for more details of these options.) When pushing Gaia to your device in this fashion, you should specify the relevant make option along with your device's scale factor, so for example *make install-gaia GAIA_DEV_PIXELS_PER_PX=1.5* for a Flame device (or 2, or 2.5, etc; see the scale factor values in the table inside 512 icon for device display.)

To check if your device is correctly connected via USB, you can type:
    $ adb devices

You should get a result like so:
    List of devices attached
    emulator-5554  device

## Building Gaia
Clone gaia:

```shell
git clone git://github.com/mozilla-b2g/gaia.git
```

While Gaia is mostly HTML/CSS/JavaScript, there are still manifest files and
configuration files that need to be built for XUL. To do this, simply type
`make` in the root of the gaia directory. This will setup XUL and process gaia
--namely all the apps in the `apps` folder. This creates a `profile` folder,
which is the build version of gaia.

To launch `b2g-bin` with your newly created profile, simply tell `b2g-bin`
about it.

```
b2g-bin -profile /path/to/gaia/profile
```

Note: `b2g-bin` requires that the path you give it to profile be a complete
one. That means either one based off of the root of the drive like
`/path/to/gaia` or based off of your home folder `~/path/to/gaia`. A path like
`../gaia/profile` will fail.

So this should load B2G and start your profile for the first time. You'll
probably get the welcoming questions like language and time-zone, as well as an
optional tour.

