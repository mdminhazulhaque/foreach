# foreach

`foreach` is a simple bash script which reads a given file/stdin line by line and executes a specific command with the line as argument.

## dude! ever heard of xargs?

Yup I did! But I am too lazy to memorize its complex argument passing techniques. So I wrote my own script.

Here is a simple example.

```bash
$ cat links.txt
https://www.youtube.com/watch?v=el0-J3tJs5M
https://www.youtube.com/watch?v=CBsXggn_4qA
https://www.youtube.com/watch?v=ZoCyVrl94yE
```

Now you want to execute `youtube-dl` with each line as argument. `xargs` might help in this case.

```bash
$ cat links.txt | xargs -L1 youtube-dl
[youtube] el0-J3tJs5M: Downloading ...
[youtube] CBsXggn_4qA: Downloading ...
[youtube] ZoCyVrl94yE: Downloading ...
```

But wait! `xargs`? `-L1`? That's too much to remember. And what if I want to use the argument in the middle of some commands? Like the following?

```bash
$ cat bands.txt
coldplay
dream theater
iron maiden
metallica
pink floyd

$ cat bands.txt | somemagiccommand echo i love {} and {} rocks
i love coldplay and coldplay rocks
i love dream theater and dream theater rocks
i love iron maiden and iron maiden rocks
i love metallica and metallica rocks
i love pink floyd and pink floyd rocks
```

`foreach` is that `somemagiccommand`. It replaces every `{}` in the command with the line from the file. You can use `{}` as many times as you want, wherever the position of the argument is.

```bash
$ cat distro.txt
kubuntu
debian
arch
freebsd
opensuse

$ foreach distro.txt echo {} is awesome # starts with {}
kubuntu is awesome
debian is awesome
arch is awesome
freebsd is awesome
opensuse is awesome
```

`foreach` can also be used without piping data into it. In such cases, the first argument will be taken as input filename. For example,

```bash
$ cat fruits.txt
apple
banana
cherry

$ foreach fruits.txt echo i love {} # ends with {}
i love apple
i love banana
i love cherry
```

You can skip `{}` if it is the only argument of the command you want to run. For example,

```bash
$ cat links.txt
https://www.youtube.com/watch?v=el0-J3tJs5M
https://www.youtube.com/watch?v=CBsXggn_4qA
https://www.youtube.com/watch?v=ZoCyVrl94yE

$ foreach links.txt youtube-dl # {} is not used here
[youtube] el0-J3tJs5M: Downloading ...
[youtube] CBsXggn_4qA: Downloading ...
[youtube] ZoCyVrl94yE: Downloading ...
```

## Disclaimer

This script relies on `[ ! -t 0 ]` to detect piped input, which may not work correctly in zsh or other non-bash, non-interactive terminals. In such environments, prefer the piped form (`cat file.txt | foreach cmd`) over the file-argument form (`foreach file.txt cmd`).

## Installation

Download `foreach` and put it anywhere in your `PATH`. Or do the following.

```bash
sudo wget -O /usr/local/bin/foreach https://raw.githubusercontent.com/mdminhazulhaque/foreach/master/foreach
sudo chmod +x /usr/local/bin/foreach
```

## Changelog

- 2026-03-27 Fixed quoting, pipe detection, and sed injection issues
- 2017-05-05 Fixed an issue where last line was not being read

## More Bugs?

Open issues if you find any. You are also welcome to submit patches.
