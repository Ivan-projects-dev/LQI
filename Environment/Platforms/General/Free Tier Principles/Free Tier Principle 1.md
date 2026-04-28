#SoftDev **Never Touch the QPU Without Simulator Pass**

Every QPU job you run should answer specific question that simulation cannot. If you can answer it in simulation, do not spend QPU time.

Questions only QPU can answer:
- What is the actual error rate on this hardware for this circuit?
- Does [[Error Mitigation]] reduce errors by the expected amount?
- Are the results consistent across different backends?

Questions simulation answers fine:
- Is my circuit logically correct?
- Does the algorithm converge to the right answer?
- Is my transpilation valid?

Establish this discipline early. Beginners who skip simulation waste most of their free credits debugging circuit bugs on the QPU.
