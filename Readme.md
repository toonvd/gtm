`gtm` is a **g**it **t**ask **m**anager (or **t**ime **m**anager, if you don't like tasks).

It is a tool for tracking time, that you spend on commits in your git repositories and managing tasks.

### How it works

Let's assume that you have a git repository, in which you'd like to start using `gtm`. Just go there and type
```bash
$ gtm init
```
it will create special git hooks in your repository and set up git config to push and fetch notes with time information.

> **Warning**: be careful if you have any custom local pre-commit, post-commit, post-checkout and post-merge hooks, because `gtm` just overwrites them

> **Note**: after this command `git push` will push timetracker notes instead of default behaviour

Now, every branch in your repository has it's own timer and they automatically switch, when you switch branches with `git checkout <branch>`. 
To view current state of timer use
```bash
$ gtm status
```
If you just did `init` and `status`, your timer is empty and turned off. To turn it on type
```bash
$ gtm start
```

Now you can just work in you repository and when you commit changes with usual `git commit` timer information will be written to the commit note and timer will be reset.

If you have a break or just want to spend some time watching cute cats on youtube, you cat pause timer with
```bash
$ gtm stop
```

If you forgot to start or stop timer properly you can use `gtm set <minutes>` or `gtm add <[-]minutes>` to adjust timer state.

To see the list of your branches and their timers you can use
```bash
$ gtm list
```

That's basically all about time tracking with `gtm`. There are some other convenient commands — you can find them with `gtm help`.

### Task management

`...`

### Installation

This tool requires installed git and [ghi tool](https://github.com/stephencelis/ghi) for dealing with github issues.

`gtm` itself is just a bash script, so you can copy it to somewhere to your `PATH` or make a link/alias/whatever.

### More information

For other commands and information use `gtm help`. If something is unclear and information is missing, please open an issue.
