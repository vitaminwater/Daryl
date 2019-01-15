# Meet Daryl

![Daryl](assets/daryl_pic.png?raw=true "Daryl")

Table of Contents
=================

   * [Intro](#intro)
   * [TODO list apps suck](#todo-list-apps-suck)
   * [How it works](#how-it-works)
      * [Installation](#installation)
      * [Orders](#orders)
         * [Create note](#create-note)
         * [(r) Get a random note](#r-get-a-random-note)
         * [(c) Retrieve current note](#c-retrieve-current-note)
            * [Force current note](#force-current-note)
         * [(--) Add log to a note](#---add-log-to-a-note)
         * [(d) Mark current note as done](#d-mark-current-note-as-done)
         * [(a) Dump all &amp; Search](#a-dump-all--search)
         * [(e) Edit current note](#e-edit-current-note)
         * [Work on another note](#work-on-another-note)
         * [(s) Sync to github](#s-sync-to-github)
      * [Android app](#android-app)

# Intro

Daryl helps you focus on multiple things, one at a time.

# TODO list apps suck

I hate TODO list apps for two simple reasons:
- I have to open it, which is usually enough for me to forget what I was about to note.
- Once openned it'll dump a river of TODOs at my face, which is enough for me to forget what I was about to note.

# How it works

## Installation

If you are already using daryl on another computer, you might want to follow the [Sync to github](#sync-to-github) section first.

Just download and place the `daryl` file in a directory like `/usr/local/bin`, and `chmod +x` it:

```sh

curl https://raw.githubusercontent.com/vitaminwater/Daryl/master/daryl -o ~/Downloads/daryl
chmod +x ~/Downloads/daryl
sudo mv ~/Downloads/daryl /usr/local/bin/

```

Actually typing `daryl` everytime is already enough for me to forget what I was about to note, so let's just add an alias so we can only type `d`:

```sh

echo "alias d='daryl'" >> ~/.bashrc

```

For macosx users, replace `.bashrc` with `.profile`.

## Orders

Daryl only responds to a restricted set of possible orders:

### Create note

Just type `d` followed by your note, no need to add ", all params are concatenated.

```sh

$ d This is noted I can forget it and go on
Creating note 1547367966.txt, sure ? (Y/n):          <----- Just type enter here 'Y' is default
Saving as /home/user/.daryl/1547283650.txt: ok

```

The name `1547283650.txt` is just the UNIX timestamp.

### (r) Get a random note

So as said above I hate seeing all TODOS, it's depressing and at one point our brain is made to start not seeing them anymore.

What I want is arrive at my desk in the morning, check if I have a task going on since last day, and if not, just ask for a random one.

```sh

$ d r
Sat Jan 12 10:00:50 CET 2019 - 1547283650.txt:
This is noted I can forget it and go on

```

You can call this as many times as you want.
And once you stop calling the random command, the last note you saw is the one chosen to work on.

### (c) Retrieve current note

Now that we have a note we're going to work on, we can see this note as many times as we need by typing:

```sh

$ d c
Sat Jan 12 10:00:50 CET 2019 - 1547283650.txt:
This is noted I can forget it and go on

```

#### Force current note

The `c` command can also be used to change the current note to a specific one. Just add the name after the `c`, like in `d c 1547244396.txt`.

### (--) Add log to a note

As you go on, you might want to add logs to a note, so next time you get on it, you can remember where you were at last time you left.

```sh

$ d -- https://the-url-with-solution.com/foo
Added log to 1547283650.txt: ok

```

The kind of things you'd want to use the logs for:
- bookmark urls
- write notes or gotchas you might get into again
- useful commands
- something like `TAG: useful_tag`, so you can find them with a `d a TAG: useful_tag`.
- Or even just a "I was here" to know when was the last time you worked on it.

Back to our note, now when we call `d c` to get the current note, we can see our newly created log:

```sh

$ d c
Sat Jan 12 10:00:50 CET 2019 - 1547283650.txt:
This is noted I can forget it and go on

====== Sat Jan 12 10:18:11 CET 2019 ======
https://the-url-with-solution.com/foo

```

### (d) Mark current note as done

And at last, we can mark the current note as done:

```sh

$ d d
The note $LAST will be ${RED}marked as done${NC}, sure ? (y/N): 
Marked note /home/user/.daryl/1547283650.txt as done: ok

```

This also adds a log to the note stating when it got done.

### (a) Dump all & Search

Just do a `d a` and you'll have all your notes dumped. Useful to find a particular one.

If a second parameter is present, it is treated as a regexp, and only the matching notes are dumped.

### (e) Edit current note

Sometime you'd like to edit your current note, just do a `d e`.

Keep in mind that it'll try to open it with the editor in the `$EDITOR` environment variable. So set it if you haven't.

### Work on another note

If you want to use another note as the current, without actually changing the current note (like, for a phone call or something), just add `FORCE_CURRENT=[note name]` just before the `d`.

```sh

FORCE_CURRENT=[note name] d

```

If you have recurrent needs on a particular note, just create an alias like `alias z='FORCE_CURRENT=[ THIS_NOTE_NAME ] d'`, now all we have to do is replace the `d` from our commands by a `z` and you're working on the other note.

To install the alias:

```sh

echo "alias z='FORCE_CURRENT=[ THIS_NOTE_NAME ] d'" >> ~/.bashrc

```

For macosx users, replace `.bashrc` with `.profile`.

### (s) Sync to github

Ok that's cool but I have multiple computers, so I want synchronization between the machines.

Oh, and I also want history of want I do, who knows what could happen, it's so easy to `rm *` by negligence.
This also doubles as some sort of `Ctrl-z`.

Good news that's not new need, so let's use `git` as a backend.

First thing is to initialize the `~/.daryl` directory as a git repository.

```sh

cd ~/.daryl
git init
git remote add origin git@your-git-server.com:daryl.git
git add .
git commit -m 'first commit'
git push --set-upstream origin master

```

Now whenever you want to sync:

```sh

$ d s
[master 6664e6e] Sync command
 1 file changed, 2 insertions(+)
 create mode 100644 1547286167.txt
Already up-to-date.
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 282 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To git.ccsas.biz:daryl.git
   4c06a1a..6664e6e  master -> master
Syncing to git@your-git-server.com:daryl.git: ok

```

This command does a pull and a push, so it's bi-directionnal.

When adding a new computer, just do a:

```sh

git clone git@your-git-server.com:daryl.git ~/.daryl

```

Before typing the first daryl command.

## Android app

Yes you heard right that's actually available on android ! Ok, not really, but still, you can do all the above on your android phone.

First step is to install the app called `Termux` on the Play store.

Once launched you just get a terminal with a `/usr` and `/home` placed inside the app's disk space, `Daryl` being purely POSIX dependant, it's all we need to get it alive.

At that point, all you need is follow the [Installation Guide](#installation), beware that you need to change `/usr` to `$HOME/../usr`.
