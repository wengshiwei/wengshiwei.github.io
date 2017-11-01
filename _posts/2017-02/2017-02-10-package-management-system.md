---
published: false

layout: post
title: Package Management System
comments: true
category:
- programming-languages
- chinese
---

# Overview #

The report is a survey of language package managers. It's consisting of four sections. In the first section we introduce package managers, some related concepts and its components. We briefly introduce some language package managers in the second section. In the third section we discuss some issues in package managers and analyse how and why existing package managers behave at aspects we discuss. The fourth section is summary.

# What is a Package Management System

Package Management System, or Package Manager, is a set of tools to get required files. Many domains have package managers, for example in operation systems level, Ubuntu have `apt-get` to maintain system packages. Program languages have package managers dedicated to projects in that language. Package managers are batteries-included for new languages. Some previous prevailing languages invents its package manager afterwards for example Composer for PHP.

We clarify that definition of package managers isn't solid. Features in some package managers may be absent in other package managers due to trade-offs or design defects. Package managers sometimes overlaps with build system, continuous integration or DevOps. Though package managers always rely on module systems in implementation, they aren't highly coupled.

# Concepts of Package Management System

## Initial introduction of Package Management System

A stand-alone unit maintained by a package manager is **a project**. A project in a full-featured package manager should contain project code, manifest files, lockfiles and dependencies files. Project code is your code. manifest files specify projects you depend on. Lockfiles contain the details metadata of your dependency, especially exact versions. Dependencies files
are the actual files you depends on. The result of package management operarions is a certain thing that a compiler can build or an interpreter can run.

Package Manager is a set of tools to facilitate developping and delivering. Almost all package managers' functions can be done without it. You can manually download your dependencies code, configure it in your project and arrange it in proper folder structure. You can send your colleagues the dependencies with a compressed file of all your code. If some functions of a language's package manager are missing, you can always fix it by manually operations.

## An Example Scenario

Developer initializes a project using a package manager. As he develops, he gets other packages from the official registry or other repositories. During the development, one of the package he uses have fixed a bug and release a updated version. He updates his dependency using the package manager. After developping he deploys the project to another server. In that server he installs all the required packages then build his project.

A manifest file records the metadata of the project. The dependency is the important section in manifest. Dependency is usually specified by an entry with at least package name and version, and a repository address if a package manager supports unofficial repositories. In the above scenario, a package usually has a new version after fixing a bug. Before deploying on a server, some package managers generate a lockfile for your project. As name suggests, a lockfile "locks" some information. In manifest the version of a package may be a range, but a lockfile specifies the exact version installed in the project. After deploying, packcage manager can install the same dependency from the lockfile.

Noting we both use the word project to depict a project with its dependencies on its manifest file and a project with its depdendencies install according to its manifest file. Before you run or compile a project, you must install dependencies from an entry on manifest files to concrete code files. If you publish you project to the official registry, you are't able to commit your dependent files because users of your project can install dependencies themselves.

## Important Components of a project besides your code

### Manifest files

A manifest file contains the metadata of your project. It tells the package manager what is the project. It lists packages your project depends on, usually with versions.

Not every package manager update the manifest proactively. For example in `npm` you can install a package and record it in the manifest or not by specifying when installing. It's not always true that manifest file is consistent with the project. Sometimes you have to edit it manually.

### Lockfiles

A lockfile records the exact infomation of your packages. In a manifest file you can have a version range of a required package while lockfile specifies the exact version it requires. Lockfiles help to achieve reproducible build, to make sure if you can build your project then other can also build it. Manifest files cannot guarantee a repoducible build. For example in the manifest you depends package `foo` which version is at least `1.2` and you install current latest version `1.2.3` when developping. However when others use your project, the latest version of `foo` is `1.2.4` but it contains a bug so your project cannot build.

### Depedency files

Depedency files are the project that your project depends on. The word depedency has two meanings, the depended package and the files in that package. The depented packages themselves may also depend on other packages. This phenomenon is called nested dependency. Package managers provides tools to add, update and remove depedency.

### Official Registries

Many package managers have official registries for their packages, with which users can search and fetch packages easily and quickly.

Depedency on official registries isn't always more secure. A famous event is a NodeJS developer unpublish his widely depended package `left-pad` due to some reasons, which break thousands other packages that depend on it. This lesson made npm community change their unpublish policy: developper can freely unpublish his packge in 24 hours and he has to contact support for next steps if more than 24 hours. Putting aside this event, `Cargo` explicitly says any publich is permanent, the version can never be overwritten, and the code cannot be deleted.

# List of Some Language Package Managers

Before delving into package managers, we list some language package managers that will be discussed in following sections.

## Ruby, RubyGem and Bundler

`RubyGem` (or `Gem`) is a package manager for Ruby with an official repository. `Bundler` is the defacto official project manager. A Gem is a project published on RubyGem. Gem's manifest file is `gemspec` file. Bundler's lockfile is `Gemfile.lock`.

## Python and pip

`Pip` is a package manager for Python. `Python Package Index (PyPI)` is the official repository. In `PIP` you can use `pip freeze` to genereate the lockfile.

## PHP and Composer

`Composer` is a package manager for PHP with an official repository. Composer's manifest file is `composer.json`. Composer's lockfile is `composer.lock`.

## JavaScript, Nodejs, npm and Yarn

`Nodejs` is a Javascript runtime built on Chrome's V8 engine. `Npm` is a package manager for Nodejs with an official repository. `Npm` isolates a project by directories. In npm you can manually lock dependency version by `npm shrinkwrap` and reduce duplication by `npm dedupe`.

`Yarn` is a new npm-compatible package managers invented by Facebook at Oct 2016. Yarn decide new commands and workflows

## Rust and Cargo

`Cargo` is a package manager for Rust with an official repository `crates.io`. Cargo's manifest file is `Cargo.toml`. Cargo's lockfile is `Cargo.lock`. Cargo update lockfile when runnning `cargo build` for your project and won't update dependencies to new version unless running `cargo update`.

A publish in crates.io is permanent. The version can never be overwritten and the code cannot be deleted. You can `yark` a project to prevent any new dependencies created against one version but all existing dependencies continue to work.

## NuGet and C#

`NuGet` is a package manager for Microsoft .Net Languages for example C#. `NuGet` is already a part of Visual Studio 2017. It also works as a commandline tool for Mono, an open source .Net framework supporting Linux and Mac OS as well.

## Go

Go has lots of alternatives of official package manager but no official one.
The language itself contains some package management design such as `GOPATH` and `vender` folder. `GOPATH` is an environment variable to isolate your project. `vender` folder is a convertion folder to put dependency.

## OCaml and OPAM

`OPAM` is official package manager for `OCaml`. Besides package management, `OPAM` support switch multiple versions of OCaml.

# Some Issues in Package Managers

## Isolation of a Project

Projects lie in file systems today. A naive way of isolation is to put your project in a separate folder. This doesn't always work if that package manager have some global state. Not all projects can isolate itself merely by putting in a separate folder. This is due to some project managers also work as system managers. They can install packages "globally for the whole system". Therefore, themselves have global states.

Isolation by folder is straightforward. Many package managers choose this for example Composer, NPM and Yarn, and Rust. These managers often offer command like "<manager> init" to initialize a project, which is very convenient.

Python isolates a project by putting it in a virtual environment. After Python 3.6, standard module `venv` is the official recommended tool for virtual environment. `Virtualenv` is the most prevailing tool to create and maintain a virtual environment.  The virtual environment in fact is a new interpreter located in a separate path and itself always created via a soft link to your real installed Python.

Go isolates projects and itself by setting environment variables such as `GOROOT` and `GOPATH`. For languages usually developped in an IDE for example VS Studio and XCode, isolation usually become trival.

Sometimes you have multiple languages simultanously for example Python 2 and Python 3, or Ruby 1.9 and Ruby 2.0. The reason is some useful packages don't support the latest version and some lagecy projects need old version. Python's virtualenv solve the problem at no extra cost since there is no differences regarding the version of the new interpreter. OPAM has command "opam switch" to choose different versions of OCaml. Some languages has dedicated versions managers for example RVM for Ruby.

## Application or Libraries

Are all projects homogeneous? The answer might be not and a pronominent dichotomy is applications and libraries. Application projects are usually installed by users while libraries are usually installed by another projects. Some project managers respect this dichotomy.

Though Ruby uses Bundler to manage either normal application projects and library projects, it assumes that an application project never be added to a gem and won't be depended by other project. A library project works as a Gem so it might be added to the registry. For an application, Bundler can lock your dependency into a lockfile. While for a library, it's recommended to use manifest file called `GemSpec` to specify dependency. You can never lock this `GemSpec`.

In Rust/Cargo, a project needs specify whether it's an application or a library in initialization. We will see the difference when talking about lockfiles.

## Dependency

### Nested Dependency and multiple versions

The dependency can be nested. The packages you project depends also depends on other packages, for example package A depends on package B and C while package B also depends on package C. If package A and C specify overlapping ranges of version, package manager can select one version that satisfies them simultanously. If package A and package B happen to depend on exactly two different versions of C, the package manager had to install instances of both versions to satisfy them. Solving shared version constraints is NP-complete. In fact, some package manager use its own heuristics searching solver for example such as Pub for Dart while some directly use external solver for example Opam for OCaml.

Not all package managers install instances of multiple versions of one package. In package managers such as Gem and NuGet, every library can have at most one version. If package managers cannot resolve the dependency, they give errors immediately. On the other hand, package managers such as PIP or Yarn, multiple versions is permitted by default. In Yarn you can force one version restriction by adding "--flat" in install command. When detect version requirement conflict, yarn prompt user to decide and remember the answers in manifest file.

### Dependency Hell

One solution that can ensure reproducible and identical build is locking every dependencies to their exact versions whether it's nested or not and permit multiple instances. It will result in tons of dependency both in amount of package and in logic. It's call dependency hell. As far as I know no package managers use this "pessimistic" solution. Instead they choose an "optimistic" solution: only respect the manifest file of nested dependency but ignore their lockfile. In other word, only the top project re-calculates dependencies for itself and nested dependencies.

### Should lockfiles be added into version control system

Should lockfiles be added into version control system? This problem reveals some fundemental consideration of package managers. Let us check how some package manager do regarding this problem.

In Rust/Cargo, lockfile is included in version control for application, but not library. If A library ends up being used transitively by several dependency paths and if every path were to check their lockfile for this library, then multiple copies of this library at different versions might be used, otherwise perhaps a version conflict happens for this library. Cargo chooses to only respect lockfile of an end application project and ignore lockfile for a dependent library porject even if exists.

Yarn requires all projects commit their lockfiles. However, Yarn ignores all lockfiles in the dependency. Yarn thinks you would never be able to update the versions of your dependency because they would be locked by other lockfile. The reason that Yarn still requires lockfile for any project is in general cases a NodeJS project has many dependency in development but only has a limit of depedency at runtime. Yarn points out a library should always be installed more frequently by users than by contributors of that library. If there were a incompatible package update, users must find it before contribute test it. In Yarn's word, it's "user testing". Yarn's philosophy is incompatible update only occurs if a user first time installs a package or user manually updates an existing dependency so it's user's duty to prevent and fix it for example rolling back the version and open an issue in the library.

PHP/Composer suggests commiting lockfile to version control. Composer can install your project whether it contains a lockfile or not. However, this lockfile will not have any effect on other projects that depend on it.

Ruby/Bundler has similar design as Rust. Gem lockfile makes your application a single package of both your code and the third-party code it ran the last time you know for sure that everything worked. Specifying exact versions of the third-party code you depend on in your Gemfile would not provide the same guarantee. An application under Bundler use Gemfile and the lockfile for it.
A gem is recommended to use a GemSpec file to resolve dependency, which cannot be used to generate a lockfile.

### Top-Level Project

Sometimes a prokect may contain several functionalities, it's a dilemma to group them as one project or to separate each as a stand-alone project. Of course either way will work, but either way has its one trouble. It's the same reason that git introduces git-submodules in later version. Some package manager offers optional dependency, then you can only use part of some packages.

## Versions

Versions are used to identify a snapshot of project in its editing history. Though in most cases version numbers and versions are interchangeable, versions are not always "numbers", for example "1.2.3.alpha" is also a valid version. some naming schemes are used to give versions more meanings. The aim of naming scheme is to establish some convertions between package developers and its users. To identify a revision of a repository on GitHub, we can also use a tag or a branch name as its version.

Many recently package managers choose `SemVer`(Semantic Versioning) as their naming scheme. A version number in SemVer is consisting of three increasing numbers: major version, minor version and patch version. The key idea of SemVer is increasing coresponding number according to separate updates: to update major version when making incompatible API change, to update minor version when add funcionality without breaking API, and to update patch version when just fixing bugs. Comparing to a naive scheme, such as to increase the unique version number when making any updates, `SemVer` add semantic meanings to version numbers.

Version range denotes a range of versions. Under SemVer, version range can contain semantic meaning. For example, when current latest version is 1.2.3 and your version range is ">=1.2.3 <1.3.0". This means the actual dependency must support all API defined in 1.2.3 without breaking the compatability since version is greater than 1.2.3. At the same time you don't want any new functionality in the package, so you require version is less than 1.3.0. If the version range is ">=1.2.3 <2." This means you don't care about whether new API is added but all API defined in 1.2.3 must work.

SemVer is currently used in many project managers might be related to the fact that lockfile of dependencies is ignored. Since the lockfile is ignored, the manifest file is the only place to express dependency. As long as two versions of one dependency is compatible, this dependency is reliable no matter which version it is. Semantic Versioning provides a convention to reflect this concern simply on version numbers.

# Summary

In this report, we discuss what is a packcage manager, its components and some design issues. Package manager is a set of tools to facilitate development in that languages. The design choices, if not defects, are based on the nature of that language and their community. To guarantee reproducible build, package managers need to generate lockifles for every packages; to minimize dependency, the top project resolve the whole project according to manifest files. In my opinion, tradeoff between these two extreme cases is the most important in package management system design.


[references]
Software Build Systems: Principles and Experience, 2nd edition, by Peter Smith
Applying Module System Research to Package Management [Tucker 01]
Engage: A Deployment Management System [Fischer 12]
https://medium.com/@sdboyer/so-you-want-to-write-a-package-manager-4ae9c17d9527
http://doc.crates.io/index.html
https://yarnpkg.com/blog/2016/11/24/lockfiles-for-all/
https://getcomposer.org/doc/02-libraries.md#lock-file
http://bundler.io/rationale.html
http://guides.rubygems.org/command-reference/
http://yehudakatz.com/2010/12/16/clarifying-the-roles-of-the-gemspec-and-gemfile/
http://guides.rubygems.org/command-reference/
https://pip.pypa.io/en/stable/reference/pip_install/#requirements-file-format
https://docs.microsoft.com/en-us/nuget/consume-packages/dependency-resolution
https://docs.npmjs.com/cli/dedupe
https://docs.npmjs.com/cli/shrinkwrap
https://en.wikipedia.org/wiki/Dependency_hell
http://semver.org/
https://blog.versioneye.com/2014/01/15/which-programming-language-has-the-best-package-manager/
https://docs.google.com/document/d/19HNnqMsETTdwwQd0I0zq2rg1IrJtaoFEA1B1OpJGNUg/edit
http://blog.ezyang.com/2014/08/the-fundamental-problem-of-programming-language-package-management/


