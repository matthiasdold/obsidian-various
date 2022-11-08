[Markmap](https://markmap.js.org/) is a a super nice tool to work with markdown based mind maps.
I have installed the cli via npm
```bash
npm install -g markmap-cli
```
See also the [github](https://github.com/markmap/markmap/tree/master/packages/markmap-cli) for it.

My usage is then to simply use a tmux split to run the following command for getting a live updating rendered version in my browser:
```bash
markmap -w <mymarkdownfile>.md
```