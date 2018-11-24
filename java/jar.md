# JAR General

## Extract a file from jar 

Suppose you want to extract the TicTacToe class file and the cross.gif image file. To do so, you can use this command:

```bash
jar xf TicTacToe.jar TicTacToe.class images/cross.gif
```

This command does two things:

1. It places a copy of TicTacToe.class in the current directory.
2. It creates the directory images, if it doesn't already exist, and places a copy of cross.gif within it.