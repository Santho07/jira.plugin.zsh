#  Jira Oh-My-ZSH plugin  #

CLI support for JIRA & Git interaction

**Note**: This plugin based on [the official Oh-My-ZSH Jira plugin](https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/jira) maintained by [tresni](https://github.com/tresni). But it can recognize an issue code in a current Git branch name. See [below](#git-branch-recognition) for more information. 

##  Description  ##

This plugin provides command line tools for interacting with Atlassian's [JIRA](https://www.atlassian.com/software/jira) bug tracking software.

The interaction is all done through the web. No local installation of JIRA is necessary.

In this document, "JIRA" refers to the JIRA issue tracking server, and `jira` refers to the command this plugin supplies.

##  Usage  ##

This plugin supplies one command, `jira`, through which all its features are exposed. Most forms of this command open a JIRA page in your web browser.

```
jira            # performs the default action

jira git        # opens an issue related to the current Git branch
jira git m      # opens an issue related to the current Git branch for adding a comment
jira new        # opens a new issue
jira dashboard  # opens your JIRA dashboard (alias: `jira dash`)
jira reported [username]  # queries for issues reported by a user
jira assigned [username]  # queries for issues assigned to a user
jira ABC-123    # opens an existing issue
jira ABC-123 m  # opens an existing issue for adding a comment
```

#### Debugging usage  ####

These calling forms are for developers' use, and may change at any time.

```
jira dumpconfig   # displays the effective configuration
```

##  Installation  ##

Create folder inside your Oh-My-ZSH directory with custom plugins. Copy plugin files to it. Activate it in your ZSH config file. Usually it looks like:

```
cd ~/.oh-my-zsh/custom/plugins/
git clone git@github.com:igoradamenko/jira.plugin.zsh.git jira
vim ~/.zshrc
```

In opened file find array `plugins` and add `jira` in it (or add whole line if it does not exist):

```
plugins=(jira)
```

After that this custom plugin will override default `jira` plugin from Oh-My-ZSH.

##  Setup  ##

The URL for your JIRA instance is set by `$JIRA_URL` or a `.jira_url` file.

Add a `.jira-url` file in the base of your project. You can also set `$JIRA_URL` in your `~/.zshrc` or put a `.jira-url` in your home directory. A `.jira-url` in the current directory takes precedence, so you can make per-project customizations.

The same goes with `.jira-prefix` and `$JIRA_PREFIX`. These control the prefix added to all issue IDs, which differentiates projects within a JIRA instance.

For example:

```
cd to/my/project
echo "https://jira.atlassian.com" >> .jira-url
```

(Note: The current implementation only looks in the current directory for `.jira-url` and `.jira-prefix`, not up the path, so if you are in a subdirectory of your project, it will fall back to your default JIRA URL. This will probably change in the future though.)

###  Variables  ###

* `$JIRA_URL` - Your JIRA instance's URL
* `$JIRA_NAME` - Your JIRA username; used as the default user for `assigned`/`reported` searches
* `$JIRA_PREFIX` - Prefix added to issue ID arguments
* `$JIRA_RAPID_BOARD` - Set to `true` if you use Rapid Board
* `$JIRA_DEFAULT_ACTION` - Action to do when `jira` is called with no arguments; defaults to `git`
* `$JIRA_BRANCH_REGEX` — Extended regular expression (ERE) for recognizing an issue code in a Git branch name; defaults to `s/.+\-([A-Z0-9]+-[0-9]+)\-.+/\1/p`

### Git branch recognition ###

By default `jira` means `jira git`. So it will try to get issue code from the current Git branch name and open it.

Set up `$JIRA_BRANCH_REGEX` in your `~/.zshrc` to change the default recognition which accepts branches like these:

```
feature-LOL-123-new-markup
bugfix-OMZSH-4224-remove-the-world
```

And so on. It means that the default pattern is `<string>-<issue code>-<string>`, where `<string>` is at least one symbol.

### Browser ###

Your default web browser, as determined by how `open_command` handles `http://` URLs, is used for interacting with the JIRA instance. If you change your system's URL handler associations, it will change the browser that `jira` uses.
