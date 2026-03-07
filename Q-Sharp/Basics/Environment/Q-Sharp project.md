#Q-Sharp 
Q# project contains Q# manifest file, `qsharp.json`, & $1>$ `.qs` files in a specified folder structure. 

When you open `.qs` file in VS Code, the Compiler searches the surrounding folder hierarchy for the manifest file & determines the project's scope. If no manifest file is found, then the Compiler operates in a single file mode.

When you set the `project_root` in a Jupyter Notebook or Python file, the Compiler looks for the manifest file in the `project_root` folder.

External Q# project is a standard Q# project that resides in another directory or on a public GitHub repository, & acts as a custom library. External project uses `export` statements to define the funcs & operations that are accessible by external programs. Programs define the external project as a dependency in their manifest file, & use `import` statements to access the items in the external project, such as operations, funcs, structs, & namespaces. For $>$ info, see Using projects as external dependencies.

Q# project is defined by the presence of manifest file, named `qsharp.json`, & a `src` folder, both of which must be in the root folder of the project. `src` folder contains the Q# source files. 
For Q# programs & external projects, the Q# Compiler detects the project folder automatically. 
For Python programs & Jupiter Notebook files, you must specify the Q# project folder with a `qsharp.init` call. However, the folder structure for Q# project is the same for all types of programs.
![[Pasted image 20260304222855.png]]