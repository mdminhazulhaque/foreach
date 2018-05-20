# foreach

`foreach` is a simple bash script which reads given file/stdin line by line and executes specific command with the line as argument.

## dude! ever heard of xargs?

Yup I did! But I am too lazy to memorize its complex argument passing techniques. So I wrote my own script.

Here is a simple example.

```bash
~:minhaz $ cat links.txt 
https://www.youtube.com/watch?v=el0-J3tJs5M
https://www.youtube.com/watch?v=CBsXggn_4qA
https://www.youtube.com/watch?v=ZoCyVrl94yE
```

Now you want to execute `youtube-dl` with each of the line as argument. `xargs` might help in this case.

```bash
~:minhaz $ cat links.txt | xargs -L1 echo youtube-dl
youtube-dl https://www.youtube.com/watch?v=el0-J3tJs5M
youtube-dl https://www.youtube.com/watch?v=CBsXggn_4qA
youtube-dl https://www.youtube.com/watch?v=ZoCyVrl94yE
```

But wait! `xargs`? `-L1`? That's too much to remember. And what if I want to use the argument in the middle of some commands? Like the following?

```bash
~:minhaz $ cat bands.txt 
coldplay
dream theater
iron maiden
metallica
pink floyd

~:minhaz $ cat bands.txt | somemagiccommand echo i love {} and {} rocks
i love coldplay and coldplay rocks
i love dream theater and dream theater rocks
i love iron maiden and iron maiden rocks
i love metallica and metallica rocks
i love pink floyd and pink floyd rocks
```

`foreach` is that `somemagiccommand`. It does format the line from a file where referenced as `{}`. You can use `{}` as much as you want, wherever the position of the argument is.

```bash
~:minhaz $ cat distro.txt 
kubuntu
debian
arch
freebsd
opensuse

~:minhaz $ foreach distro.txt echo {} is awesome # starts with {}
kubuntu is awesome
debian is awesome
arch is awesome
freebsd is awesome
opensuse is awesome
```

`foreach` can also be used without piping data into it. In such cases first argument will be taken as input filename. For example,

```bash
~:minhaz $ cat fruits.txt 
apple
banana
cherry

~:minhaz $ foreach fruits.txt echo i love {} # ends with {}
i love apple
i love banana
i love cherry
```

You can skip `{}` if it will be passed as only one argument of the command you want to run. For example,

```bash
~:minhaz $ cat links.txt 
https://www.youtube.com/watch?v=el0-J3tJs5M
https://www.youtube.com/watch?v=CBsXggn_4qA
https://www.youtube.com/watch?v=ZoCyVrl94yE

~:minhaz $ foreach links.txt youtube-dl # {} is not used here
[youtube] el0-J3tJs5M: Downloading ...
[youtube] CBsXggn_4qA: Downloading ...
[youtube] ZoCyVrl94yE: Downloading ...
```

## Installation

Download `foreach` and put anywhere in your `PATH`. Or do the following.

```bash
sudo wget -O /usr/local/bin/foreach https://raw.githubusercontent.com/mdminhazulhaque/foreach/master/foreach
sudo chmod +x /usr/local/bin/foreach
```

## Changelogs

- [x] 2017-05-05 Fixed an issue where last line was not being read


## More Bugs?

Open issues if you find any. You are also welcome to submit patches.
