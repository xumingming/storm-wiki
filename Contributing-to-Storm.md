### Getting started with contributing

Some of the issues on the [issue tracker](https://github.com/nathanmarz/storm/issues) are marked with the "Newbie" label. If you're interesting in contributing to Storm but don't know where to begin, these are good issues to start with. These issues are a great way to get your feet wet with learning the codebase because they require learning about only an isolated portion of the codebase and are a relatively small amount of work.

### Learning the codebase

The [[Implementation docs]] section of the wiki gives detailed walkthroughs of the codebase. Reading through these docs is highly recommended to understand the codebase.

### Contribution process

Contributions to the Storm codebase should be sent as GitHub pull requests. If there's any problems to the pull request we can iterate on it using GitHub's commenting features.

For small patches, feel free to submit pull requests directly for them. For larger contributions, please use the following process. The idea behind this process is to prevent any wasted work and catch design issues early on:

1. Open an issue on the [issue tracker](https://github.com/nathanmarz/storm/issues) if one doesn't exist already
2. Comment on the issue with your plan for implementing the issue. Explain what pieces of the codebase you're going to touch and how everything is going to fit together.
3. Storm committers will iterate with you on the design to make sure you're on the right track
4. Implement your issue, submit a pull request, and iterate from there.

### Modules built on top of Storm

Modules built on top of Storm (like spouts, bolts, etc) that aren't appropriate for Storm core can be done as your own project or as part of [storm-contrib](https://github.com/nathanmarz/storm-contrib). To be part of storm-contrib just send an email to the mailing list proposing to add your module to storm-contrib. Then you'll be added as a committer to storm-contrib and can maintain your project there. The advantage of hosting your module in storm-contrib is that it will be easier for potential users to find your project.

### Contributing documentation

Documentation contributions are very welcome! The best way to send contributions is as emails through the mailing list.

### Contributor agreement

To contribute to Storm, you will need to sign a contributor agreement. This is a standard agreement used by most major open source projects to provide legal protection to the project and community. Basically you just agree to share ownership of the code with the project.

You can download the contributor agreement [here](https://dl.dropbox.com/u/133901206/storm-apache-style-cla.txt). The easiest way to fill it out is with [HelloFax](https://www.hellofax.com/), where you can fill in the fields, sign it, and email it back all digitally. The whole process should only take a couple minutes. Email the filled and signed agreement to nathan.marz@gmail.com. 

Make sure you fill in all the fields. These are:

 1. Username for Storm
 2. Username for storm-contrib (should be same as above)
 3. Name
 4. Company's name
 5. Mailing address
 6. Telephone and email
 7. Signature
 8. Date
