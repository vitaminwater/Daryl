# Note

Note allows to take note from the terminal byte typing `note This is noted I can forget it and go on`.

# TODO list apps suck

I hate TODOlist apps for two simple reasons:
- I have to open it, which is usually enough for me to forget what I was about to note.
- Once openned it'll dump a river of TODOs at my face, which is enough for me to forget what I was about to note.

# How it works

## Installation

Just download and place the `note` file in a directory like `/usr/local/bin`, and `chmod +x` it:

```sh

curl github.com -o ~/Downloads/note
chmod +x ~/Downloads/note
sudo mv ~/Downloads/note /usr/local/bin/

```

## Usage

There's just 5 commands right now:

### Create note

Just type `note` followed by you note, no need to add ", all params are concatenated.

```sh

$ note This is noted I can forget it and go on
Saving as /home/user/.notes/1547283650.txt: ok

```

The name is just the UNIX timestamp.

### Get a random note

So as said above I hate seeing all notes, it's depressing and at one point our brain is made to start not seeing them anymore.

What I want is arrive at my desk in the morning, check if I have a task going since last day, and if not, just ask for a random one.

```sh

$ note r
Sat Jan 12 10:00:50 CET 2019 - 1547283650.txt:
This is noted I can forget it and go on

```

You can call this as many times as you want. Everytime you do it, the shown note is marked as being the `last`. Which means once you stop calling it the last one you saw is the one you choose to do.

### Retrieve current note

Now that we have a note we're going to work on, it's saved as being the `last`

You can see this note as many times as you need by typing:

```sh

$ note l
Sat Jan 12 10:00:50 CET 2019 - 1547283650.txt:
This is noted I can forget it and go on

```

### Add log to a note

As you go on, you might want to add logs to a note, so next time you get on it, you have can remember where you were. Or even just bookmark an url or something..

```sh

note -- https://the-url-with-solution.com/foo
Added log to 1547283650.txt: ok

```

and now, when we call `note l` to get the last back:

```sh

$ note l
Sat Jan 12 10:00:50 CET 2019 - 1547283650.txt:
This is noted I can forget it and go on

====== Sat Jan 12 10:18:11 CET 2019 ======
https://the-url-with-solution.com/foo

```

### Mark last note as done

And at last, we can mark the current note as done:

```sh

$ note d
This will delete note 1547283650.txt, sure ? (y/N): y
Marked note /home/stant/.notes/1547283650.txt as done: ok

```
