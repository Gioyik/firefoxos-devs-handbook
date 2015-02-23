# Working on Gaia

## Getting Gaia

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

### Stingray Build (TV layout)
The Stingray project is an initiative to enable Firefox OS platform to run on larger screen TV 

To build:
* make GAIA_DEVICE_TYPE=tv