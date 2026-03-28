#Quantum #Q-Sharp 
By default, Q# programs in Jupyter Notebooks use the `ipykernel` Python package. To add Q# code to a notebook cell, use the `%%qsharp` command, which is enabled with the `qsharp` Python package, followed by your Q# code.

When using `%%qsharp`, keep the following in mind:
- You must first run `from qdk import qsharp` to enable `%%qsharp`.
- `%%qsharp` scopes to the notebook cell in which it appears & changes the cell type from Python to Q#.
- You can't put Python statement before or after `%%qsharp`.
- Q# code that follows `%%qsharp` must adhere to Q# syntax. For example, use `//` instead of `#` to denote comments and `;` to end code lines.