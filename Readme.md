`gtm` is a **g**it **t**ask **m**anager (or **t**ime **m**anager, if you don't like tasks).

It is a tool for tracking time, that you spend on commits in your git repositories and managing tasks.

## Usage

### Time tracking

Let's assume that you have a git repository, in which you'd like to start using `gtm`. Just go there and type
```bash
$ gtm init
```
this will create special git hooks in your repository and set up git config to push and fetch notes with time information.

> **Warning**: be careful if you have any custom local pre-commit, post-commit, post-checkout and post-merge hooks, because `gtm` just overwrites them

> **Note**: after this command `git push` will push timetracker notes instead of default behaviour. Consider to use `gtm push` which pushes _both_ current branch and timetracker notes.

Now, every branch in your repository has its own timer and they automatically switch, when you switch branches with `git checkout <branch>`. 
To view current state of timer use
```bash
$ gtm status
```
If you just did `init` your timer is empty and turned off. To turn it on type
```bash
$ gtm start
```

Now you can just work in you repository and when you commit changes with usual `git commit` timer information will be written to the commit note and timer will be reset.

If you have a break or just want to spend some time watching cute cats on youtube, you can pause timer with
```bash
$ gtm stop
```

If you forgot to start or stop timer properly you can use `gtm <[+/-]minutes>` to adjust timer state.

To see the list of your branches and their timers you can use
```bash
$ gtm list
```

That's basically all about time tracking with `gtm`. There are some other convenient commands — you can find them with `gtm help`.

#### Different git repositories

When you work on several projects simultaneously, you don't need to stop one timer and start another to switch between them. Just go to the repository where you want to work and type `gtm start` — it will automatically pause any other timer and start a new one. Also if you have a break, you don't need to search, where is that running timer to stop it — `gtm stop` works globally, from any place, even not a git repository.

### Task management

The idea is very simple:
- Project should be split into small tasks
- Every task consists of
  + _GitHub issue_ containing task description and discussion with team if needed
  + _Git branch_ containing task solution, i.e. all commits related to the task
  + _Time tracker_ helping you control how much time you spend on solving the task

Separate branch for each task — almost like in Git Flow, but less strict, the main point is in binding these special branches and GitHub issues. This way you can think of your issues as a to-do list (+ just any general discussions) and `gtm` will help you work on each of them.

To create a new task you can use
```bash
$ gtm task feature/topic/foo -m "Task short description"
```
it will create a branch `feature/topic/foo` and an issue with the title `Task short description` and tags `feature` and `topic`. This prefix `feature/topic/` is unnecessary and you can use any branch name, it's just convenient, because it helps you to categorize branches and `gtm` will treat prefix as set of tags for the issue.

If you already have a branch and want to bind and issue to it just do the same (you can use `-` instead of branch name to use current one). Vice versa, if you already have an issue and want to create branch for solving it use `-i <issue_number>` instead of `-m "..."`.

When you made some commits to the task branch and pushed them to remote you should do
```bash
$ gtm connect
```
it will make a pull-request from the bound issue, so that you will see all your commits related to this task in that issue.

Now you can use `gtm info` to view the issue bound to the current task branch (with `-w` flag it will open it in your web browser). Also using `gtm list` you can see list of you branches with corresponding issue numbers. And when you want to switch between them, it may be convenient to use those numbers instead of typing branch names: `gtm switch <number>` will do that for you.

When you finished work on the task you can use `gtm close`. It will show you which steps it's going to do and ask for the confirmation. See `gtm help close` for more information.

## General usage

I use this tool for tracking time on my work for any kinds of tasks — _not only for programming_. Every project has its own git repository and when I start doing something I create a task with `gtm task ...`. 

For example, we plan a meeting: somebody creates an issue mentioning others and suggesting time suitable for everybody. The following steps are

- create a task and connect it to that issue
- start timer right before going to the meeting and stop it after it's ended
- make an empty commit like `git commit --allow-empty -m "Meeting with @mrproper and @bert: value of the universe"`
- push it and do `gtm connect`
- others also do time tracking commits to this branch, push them and comment on meeting summary in the issue, if it's needed
- somebody does `gtm close`, when discussion is finished

I use similar scenario when I need to read a paper or do any other task. Actually, the only difference with the usage for programming is that here I do empty commits. This way you can have a nice git history with all you work activities and records about spent time and you can use it to generate a report of arbitrary form (see `gtm help report` and `gtm help weekly-report`).

## Configuration

### Installation

This tool requires installed git and [ghi tool](https://github.com/stephencelis/ghi) for dealing with github issues.

`gtm` itself is just a bash script, so you can copy it to somewhere in your `PATH` or make a link/alias/whatever. Like this:
```bash
$ git clone https://github.com/laughedelic/gtm.git
$ sudo ln -s gtm/gtm ~/bin/gtm
```

### Initializing timetracker

When you create a new git repository you need at least one commit to start using timetracker. So a usual scenario (for a new repo) would be something like

```bash
$ git init
$ git add .
$ git commit -a -m "Init commit"
$ gtm init
$ gtm start +5
$ gtm amend
```

i.e. you first make an initial commit and then assign time to it with `gtm amend`.

### Timer information in the right prompt

To see concise timer information without using `gtm status` all the time, you can place it in your shell prompt!

![](http://i.imgur.com/NRdzXwF.png?1)

#### zsh

Somewhere in your zsh theme configuration:
```bash
setopt PROMPT_SUBST
RPROMPT='%15{$(gtm prompt-status)%}'
```

#### fish

Just type in the shell
```bash
function fish_right_prompt
   gtm prompt-status
end
```


## More information

For other commands and information use `gtm help`. If something is unclear or information is missing, please open an issue and I will extend documentation.

Originally, it was a fork of [gtt (git time tracker) tool](http://gitorious.org/gtt/gtt) by Daniel Garcia Moreno and Eduardo Robles Elvira, but then I think, I've rewritten it almost from scratch.

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/27313b5f86976621be0037ff3a5b15f9 "githalytics.com")](http://githalytics.com/laughedelic.github.io/gtm)

<a href="http://twitter.com/home/?status=Thanks @laughedelic for making gtm: https%3A%2F%2Fgithub.com%2Flaughedelic%2Fgtm"><img src="https://s3.amazonaws.com/github-thank-you-button/thank-you-button.png" alt="Say Thanks" /></a>
