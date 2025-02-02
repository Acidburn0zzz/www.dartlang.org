---
layout: tutorial
title: "Install Shared Packages"
description: "Packages are bundles of source code, tools, and resources that help you to organize and share code"
has-permalinks: true
tutorial:
  id: packages
next: using-polymer/
next-title: "Use Polymer Elements"
prev: remove-elements/
prev-title: "Remove DOM Elements"
---

{% capture whats_the_point %}

* Following a few conventions, such as having a valid pubspec.yaml file,
  makes your app a package.
* Use Stagehand to generate starting files for your app.
* Use `pub get` to download packages.
* pub.dartlang.org is the primary public repository for Dart packages.

{% endcapture %}

{% capture sample_links %}

<p>
Get the vector_math package and many others from:</p>

<ul>
  <li>
    <a href="https://pub.dartlang.org/"
       target="_blank">pub.dartlang.org</a>
  </li>
</ul>

{% endcapture %}

{% capture content %}

<div class="tute-target-title">
<h1>{{page.title}}</h1>
<h3>Borrow and share code.</h3>
</div>

Now that you're able to create and run a Dart application
and have a basic understanding of DOM programming,
you are ready to leverage code written by other programmers.
Many interesting and useful packages of reusable Dart code
are available at the
<a href="https://pub.dartlang.org/">pub.dartlang.org</a>
repository.

This tutorial shows you how to use `pub`&mdash;a package manager
that comes with Dart&mdash;to
install one of the packages in the repository,
the vector_math package.
You can follow these same steps to install any package hosted at
<a href="https://pub.dartlang.org/">pub.dartlang.org</a>;
just change the package name when you get to that step.
This tutorial also describes some of the resources you can expect to find
in a well-built package.

* [About the pubspec.yaml file](#about-pubspec)
* [Name the package dependencies](#name-dependencies)
* [Install the package dependencies](#install-dependencies)
* [What did you get (and not get)?](#about-packages)
* [Import libraries from a package](#use-package)
* [Other resources](#other-resources)
* [What next?](#what-next)

##About the pubspec.yaml file {#about-pubspec}

To use an external package,
your application must itself be a package.
Any application with a valid pubspec.yaml file in its top-level directory
is a package and can therefore use external packages.

You can use the Stagehand tool to generate packages
with valid pubspec.yaml files and directory structures.
Stagehand works either at the command line or (behind the scenes) in an IDE,
such as WebStorm.

Install Stagehand using [pub global activate](/tools/pub/cmd/pub-global.html):

{% prettify huge %}
$ pub global activate stagehand
{% endprettify %}

Now run the `stagehand` command to see what kinds of template files
it can generate:

{% prettify huge %}
$ stagehand
{% endprettify %}

You'll see a list of generators, including various web and server apps.
One of the web app generators is named **web-simple**.

In a new directory named `vector_victor`,
use Stagehand to generate a bare-bones web app:

{% prettify huge %}
$ mkdir vector_victor
$ cd vector_victor
$ stagehand web-simple
{% endprettify %}

The pubspec.yaml file contains the package specification written in YAML.
(Visit <a href="/tools/pub/pubspec.html">Pubspec Format</a>
for in-depth coverage.)
The contents of your pubspec.yaml file should look something like this:

{% comment %}
NOTE: I used lang-html because you seem to need either that or lang-dart
to get the colors right for the highlighted text. Also, lang-yaml doesn't
treat the URL too well.
{% endcomment %}
<pre class="prettyprint lang-html allow-scroll">
<a href="#" class="dart-popover" data-toggle="popover" data-html="true" data-trigger="hover focus" data-content="Package name (required)">name: 'vector_victor'</a>
version: 0.0.1
description: An absolute bare-bones web app.
...
<a href="#" class="dart-popover" data-toggle="popover" data-html="true" data-trigger="hover focus" data-content="List of required packages">dependencies:
  browser: '>=0.10.0 &lt;0.11.0'</a>
</pre>

The package **name** is required.
Because all web apps depend on the browser package,
`browser` is listed under **dependencies**.


##Name the package dependencies {#name-dependencies}

To use an external library package,
you need to add the package to your
application's list of dependencies
in the pubspec.yaml file.
Each item in the dependencies list
specifies the name and version
of a package that your application uses.

Let's make the vector_victor application have a dependency
on the vector_math package,
which is available at pub.dartlang.org.

1. Get the current installation details for the package:

   <ul type="a">
   <li> Go to
   <a href="https://pub.dartlang.org/packages/vector_math"
   target="_blank">vector_math's pub.dartlang.org entry</a>.
   </li>

   <li> Click the <b>Installing</b> tab.
   </li>

   <li> Copy the <b>vector_math</b> line from
   the sample <b>dependencies</b> entry.
   The entry should look something like this:

   {% prettify html %}
dependencies:
  [[highlight]]vector_math: "^1.4.3"[[/highlight]]
   {% endprettify %}
   </li>

2. Edit `pubspec.yaml`.

3. In the dependencies section,
   add the string you copied from pub.dartlang.org.
   Be careful to keep the indentation the same;
   YAML is picky!
   For example:

   {% prettify html %}
dependencies:
  browser: '>=0.10.0 <0.11.0'
  [[highlight]]vector_math: "^1.4.3"[[/highlight]]
   {% endprettify %}

See [Pub Versioning Philosophy](/tools/pub/versioning.html)
for details of what version numbers mean,
and how you can format them.

<a href="https://pub.dartlang.org/">pub.dartlang.org</a>
is the primary public repository for Dart packages.
`pub` automatically checks that
website when resolving package dependencies.
To use one of the packages from that site,
you can specify it by its simple name,
as we have done here.

##Install the package dependencies {#install-dependencies}

If you're using an IDE or Dart-savvy editor to edit `pubspec.yaml`,
it might automatically install the packages your app depends on.

If not, do it yourself by running
[pub get](/tools/pub/cmd/pub-get.html):

{% prettify none %}
$ [[highlight]]pub get[[/highlight]]
Resolving dependencies... (1.4s)
+ browser 0.10.0+2
+ vector_math 1.4.3
Downloading vector_math 1.4.3...
Changed 2 dependencies!
Precompiling executables...
Loading source assets...
$
{% endprettify %}

The `pub get` command installs the
packages in your app's dependencies list.
Pub works recursively;
if an included package has dependencies, those packages are installed as well.

Pub puts the package files under directories called `packages`.
Each package can contain libraries and other assets.

If you open your app's top-level `packages` directory,
you'll find the `vector_math` link,
which points to the Dart libraries from the vector_math package.

Pub creates a file called `pubspec.lock`,
which identifies the specific versions of the packages that were installed.
This helps to provide a stable development environment.
Later you can modify the version constraints and use `pub upgrade`
to update to new versions as needed.

##What did you get (and not get)? {#about-packages}

Besides the Dart libraries,
the vector_math package has other resources that might be useful to you
that do not get installed into your application directory.
Let's take a step back for a moment to look at what
you got and where it came from.

To see the contents of the vector_math package,
visit the
<a href="https://github.com/johnmccutchan/vector_math" target="_blank">
Dart vector math repository
</a>
at github.
Although many files and directories are in the repository,
only one, `lib`, was installed when you ran pub get.

<div>
  <hr>
  <div class="row">
    <div class="col-md-3">
    <img class="scale-img-max" src="images/libraries-folder.png"
         alt="Dart libraries directory"/>
    </div>
    <div class="col-md-7">
      <em>Dart libraries:</em>
      The lib directory contains one or more Dart libraries,
      which can be imported into your Dart programs.
    </div>
  </div>
  <hr>
  <div class="row">
    <div class="col-md-3">
    <img class="scale-img-max" src="images/housekeeping-files.png"
         alt="Housekeeping files"/>
    </div>
    <div class="col-md-7">
      <em>Housekeeping files:</em>
      When using a package written by someone else,
      the README file is a good place to start.
      It should contain important information about the package,
      such as its intent, contents, samples, and instructions.
      The LICENSE file provides copyright and rules-of-use information.
      These files can be found at the package repository.
      They are not installed when you install a package.
    </div>
  </div>
  <hr>
  <div class="row">
    <div class="col-md-3">
    <img class="scale-img-max" src="images/other-folders.png"
         alt="Document, scripts, tests, and other resources"/>
    </div>
    <div class="col-md-7">
      <em>Other resources:</em>
      Along with Dart libraries,
      a package might also contain other resources
      such as example code, tests, scripts, and documentation.
      If a package contains these resources,
      they should be in the directories as specified in the pub
<a href="/tools/pub/package-layout.html">conventions</a>.
    </div>
  </div>
  <hr>
</div>

##Import libraries from a package {#use-package}

Now that you've installed the package,
you can import its libraries and use them in your Dart file.

As with the SDK libraries,
use the **import** directive to use code from an installed library.
The Dart SDK libraries are built in and
are identified with the special `dart:` prefix.
For external libraries installed by pub,
use the `package:` prefix.

<ol>
  <li>
  Get the import details for the package's main library:

  <ol type="a">
  <li> Go to
  <a href="https://pub.dartlang.org/packages/vector_math"
  target="_blank">vector_math's pub.dartlang.org entry</a>.
  </li>

   <li> Click the <b>Installing</b> tab.
   </li>

   <li> Copy the <b>import</b> line.
   It should look something like this:

{% prettify dart %}
import 'package:vector_math/vector_math.dart';
{% endprettify %}
   </li>
  </ol>
  </li>

  <li>
  Edit your main Dart file (web/main.dart).
  </li>

  <li>
  Import the library from the package.
  By convention, package imports appear after dart:* imports:

{% prettify dart %}
import 'dart:html';

[[highlight]]import 'package:vector_math/vector_math.dart';[[/highlight]]
{% endprettify %}
  </li>
</ol>

<aside class="alert alert-info" markdown="1">
**Note:**
You specify the filename, not the library name,
when you import a library.
</aside>


##Other resources

* Dart developers share packages at
  [pub.dartlang.org](https://pub.dartlang.org/).
  Look there for packages that might be useful to you,
  or share your own Dart packages.
* See the [pub documentation](/tools/pub/)
  for more information on using and sharing packages.


##What next? {#what-next}

One package at pub.dartlang.org is polymer.dart,
which makes writing web applications easier
with data binding, templates, and declarative event handlers.
Check out the next tutorial,
[Use Polymer Elements](/docs/tutorials/using-polymer),
for an introduction to polymer.dart.


{% endcapture %}

{% include tutorial.html %}
