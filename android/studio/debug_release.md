
## Android debug和release模式的区别

“debug” 和“release” 是 Android 中默认两种构建方式。


Debug：调试版本，包含调试信息，所以容量比Release大很多，并且不进行任何优化（优化会使调试复杂化，因为源代码和生成的指令间关系会更复杂），便于程序员调试。Debug模式下除了生成可执行文件外，还有一个.pdb文件，该文件记录了代码中断点等调试信息。

Release：发布版本，不对源代码进行调试，编译时对应用程序的速度进行优化，使得程序在代码大小和运行速度上都是最优的。（调试信息可在单独的PDB文件中生成）。Release模式下生成一个文件可执行文件。

Debug和Release调用两个不同的底层库。

Debug是调试版本，包括的程序信息更多。

只有Debug版的程序才能设置断点、单步执行、使用TRACE/ASSERT等调试输出语句。

Release不包含任何调试信息，所以体积小、运行速度快。


