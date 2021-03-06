# Releasing a new version.

The steps taken for slicing an official release of Apktool.

### Taging the release.

Inside `build.gradle` there are two lines.

    apktoolversion_major
    apktoolversion_minor

The major variable should be left unchanged. If done correctly, it will already be the version
you are about to release. In this case `2.2.2`. The minor variable should read `SNAPSHOT` as
the `2.2.2` release up until this point was `SNAPSHOT` releases (Unofficial).

We need to remove the `SNAPSHOT` portion and leave the minor version blank. An example can be
found [here](https://github.com/iBotPeaches/Apktool/commit/96b70d0be7513c5a1e5d3a3b9a75e4e2b076ad79).

After we remove `SNAPSHOT` we need to make the version commit. Organization and following patterns
is crucial here. This commit should have 1 change only - the change above. Now commit this change
with the commit message - `version bump (x.x.x)`.

At this point we now have the commit of the release, but we need to tag it using the following message.

    git tag -a vx.x.x -m "changed version to vx.x.x"

For example for the `2.2.1` release.

    git tag -a v2.2.1 -m "changed version to v2.2.1"

### Building the binary.

In order to maintain a clean slate. Run `gradlew clean` to start from a clean slate. Now lets build
the new version. We should not have any new commits since the tagged commit.

    ./gradlew build fatJar proguard release

The build should tell you what version you are building and it should match the commits you made previously.

    ➜ Apktool git:(master) ./gradlew build fatJar proguard release
    Building RELEASE (master): 2.2.2

### Testing the binary.

Now the release binary is built in the same location as all other builds. Run this version against
some of the fixed bugs in this release. This is a simple test to ensure the build had no errors.

Copy the jar to any location to prep for uploading. The pattern we name the jars is

    apktool_x.x.x.jar

Or in the case of the last release - `apktool_2.2.1.jar`

Once you have the jar in this form. Record the md5 hash of it. This can be done using `md5sum`.

This can be shown for the `2.2.2` release like so

    ➜  Desktop md5sum apktool_2.2.2.jar
    1e6be08d3f9bb4b442bb85cf4e21f1c1  apktool_2.2.2.jar

Remember this hash. This is the local hash. This is our master hash. All others (Bitbucket, Backup)
must match this.

### Lets get uploading.

Lets make sure we actually pushed these release changes to the repo (Both Github & Bitbucket)

    git push origin master
    git push origin vx.x.x

    git push bitbucket master
    git push bitbucket vx.x.x

We upload the binaries into 3 places.

1. [Bitbucket Downloads](https://bitbucket.org/iBotPeaches/apktool/downloads)
2. [Github Releases](https://github.com/iBotPeaches/Apktool/releases) - Since `2.2.1`.
3. [Backup Mirror](http://connortumbleson.com/apktool/)

#### Bitbucket

This one is pretty easy. Head to the URL attached to the hyperlink #1 above. There will be a "Add Files"
button on the top right of the page. Upload the `apktool_x.x.x.jar` file.

After it is uploaded. Immediately visit the page and download it. Check the `md5` for a match.

#### GitHub

This option will not work until the tag is pushed. You can head to this [page](https://github.com/iBotPeaches/Apktool/releases/new)
to draft a new release. The `Tag version` dropdown will have the new tag. In this case `v2.2.2`.

Select that option and make the title `Apktool vx.x.x`. There will be a description field on this release.
Hold tight, we link the release blog post in this field, but we can edit the release after the fact to add this.

Upload the binary `apktool_x.x.x.jar` and submit the release.

#### Backup Server

Access to this server is probably limited so this option may not be possible. SSH into the
`connortumbleson.com` server with username `connor`. Head to `public_html/apktool` and upload
the `apktool_x.x.x.jar` to it.

Now re-generate the md5 hashes for these files.

    md5sum *.jar > md5.md5sum

Check the `md5.md5sum` file for the hashes. The file will look something like this.

    6de3e097943c553da5db2e604bced332  apktool_1.4.10.jar
    ...
    1e6be08d3f9bb4b442bb85cf4e21f1c1  apktool_2.2.2.jar

The hashes match so we have uploaded the binaries to all 3 locations. Time to get writing the release
post.

We currently blog the releases on the [Connor Tumbleson personal blog](https://connortumbleson.com/).
This may change and the formatting of these release posts change over time.

Some recent releases for understanding the pattern can be found below.

1. [2.2.1](https://connortumbleson.com/2016/10/18/apktool-v2-2-1-released/)
2. [2.2.0](https://connortumbleson.com/2016/08/07/apktool-v2-2-0-released/)
3. [2.0.2](https://connortumbleson.com/2015/10/12/apktool-v2-0-2-released/)
4. [2.0.0](https://connortumbleson.com/2015/04/20/apktool-v2-0-0-released/)

So write the post. I tend to always include the following:

1. Image of release for featured image when reshared on socials.
2. Quick sentence or two for SEO to describe the meat of this release.
3. Commit count and total for this release with author names.
4. Changelog linking to the bugs that were fixed.
5. Download including the md5 hash.
6. Link dump to Project Site, GitHub, Bug Tracker and XDA Thread.

Now that you've written this post. We need to go post it in places and update places where
Apktool is released.

### XDA Thread

We have a [thread](https://forum.xda-developers.com/showthread.php?t=1755243) on XDA Developers.
This thread follows the same pattern for all releases.

When writing a response to the XDA thread we follow another pattern of release notes. These examples
can be found below:

1. [2.2.2](https://forum.xda-developers.com/showpost.php?p=70687935&postcount=4635)
2. [2.2.1](http://forum.xda-developers.com/showpost.php?p=69188139&postcount=4478)
3. [2.0.0](http://forum.xda-developers.com/showpost.php?p=60255972&postcount=3063)

### Apktool Website

The Apktool project website has a few locations to update:

1. The homepage intro
2. The download link in header
3. The changelog page
4. The footer of homepage with history of releases.

The easiest way to describe this is to just link to a [previous release](https://github.com/iBotPeaches/Apktool/commit/5ef77bf01cf3625cb1dd1981234b3854b02496e2).

### Update Milestones

Now that we've released a version, we should hopefully have no more tickets in the release just published.
If there are, move those tickets to the next milestone.

You can head to [milestones](https://github.com/iBotPeaches/Apktool/milestones) to close the just
released version and create another.

I tend to create the next release (In this case `2.2.3`) with an ETA of 3 months in the future. This
is just a guideline but helps me to release a new version every 3 months.

### Social Spam

The final step is to send this release into the wild via some social posting. Head to the blog
where the release post was and send that link to Twiter, Google and whatever else you use.

Relax and watch the bug tracker.

# Building aapt binaries.

The steps taken for building our modified aapt binaries for apktool.

### Getting the modified `frameworks/base` repo.
First step is using the [platform_frameworks_base](https://github.com/iBotPeaches/platform_frameworks_base) repo.

While previously unorganized, the repo now follows the branch naming convention depending on the current Android version.
So `apktool_7.1` corresponds to the 7.1 Android release. This branch should work for all `android-7.1.x` tags for AOSP.

We didn't follow this naming convention until Android 7.1. So don't go looking for older versions.

This repo has a variety of changes applied. These changes range from disabling optimizations to lessening the rules
that aapt regularly has. We do this because apktool's job is to not fix apks, but rather keep them as close to the
original as they were.

### First we need the AOSP source

As cheesy as it is, just follow this [downloading](https://source.android.com/source/downloading.html) link in order
to get the source downloaded. This is no small download, expect to use 40-60GB.

After that, you need to build AOSP via this [documentation](https://source.android.com/source/building.html) guide. Now
we aren't building the entire AOSP package, the initial build is to just see if you are capable of building it.

We check out a certain tag. Currently we use `android-7.1.1_r4`.

### Including our modified `frameworks/base` package.

There is probably a more automated way to do this, but for now just remove all the files in `frameworks/base`. Now
you can clone the modified repo from first step into this directory.

### Building the aapt binary.

The steps below are different per flavor and operating system. For cross compiling the Windows binary on Unix,
we lose the ability to quickly build just the aapt binary. So the Windows procedure builds the entire Sdk.

#### Unix 32
1. `source build/envsetup.sh`
2. `lunch sdk-eng`
3. `make OUT_DIR=out-x32 LOCAL_MULTILIB=32 USE_NINJA=false aapt`
4. `strip out-x32/host/linux-x86/bin/aapt`

#### Unix 64
1. `source build/envsetup.sh`
2. `lunch sdk-eng`
3. `make OUT_DIR=out-x64 LOCAL_MULTILIB=64 USE_NINJA=false aapt`
4. `strip out-x64/host/linux-x86/bin/aapt`

#### Windows
1. `source build/envsetup.sh`
2. `lunch sdk-eng`
3. `make PRODUCT-sdk-win_sdk USE_NINJA=false`
4. `strip out/host/windows-x86/bin/aapt.exe`

#### Mac 32
1. `source build/envsetup.sh`
2. `lunch sdk-eng`
3. `make OUT_DIR=out-x32 LOCAL_MULTILIB=32 USE_NINJA=false aapt`

#### Mac 64
1. `source build/envsetup.sh`
2. `lunch sdk-eng`
3. `make OUT_DIR=out-x64 LOCAL_MULTILIB=64 USE_NINJA=false aapt`
