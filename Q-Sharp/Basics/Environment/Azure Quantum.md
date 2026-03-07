#Quantum #Cloud #Q-Sharp 
When you compile & run a quantum program, the QDK creates instance of the quantum simulator & passes the Q# code to it. The simulator uses the Q# code to create [[Qubits]] (simulations of quantum particles) & apply transformations to modify their state. Results of the quantum operations in the simulator are then returned to the program. Isolating the Q# code in the simulator ensures that the algorithms follow the laws of quantum physics & can run correctly on quantum computers.

Once you've tested your program in simulation, you can run it on real quantum hardware. When you run a quantum program in Azure Quantum, you create & run **job**. To submit job to the Azure Quantum providers, you need an Azure account & quantum workspace.

Azure Quantum offers some of the most compelling & diverse quantum hardware. See Quantum computing providers for the current list of supported hardware providers.

Once you submit your job, Azure Quantum manages the job lifecycle, including job scheduling, execution, & monitoring. You can track the status of your job & view the results in the Azure Quantum portal.