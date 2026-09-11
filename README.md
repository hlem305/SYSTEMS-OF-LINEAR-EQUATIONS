# SYSTEMS-OF-LINEAR-EQUATIONS
import numpy as np
# حل نظام معادلات: 2x + y = 8  و  x - 3y = -3
A = np.array([[2, 1], [1, -3]])
B = np.array([8, -3])
X = np.linalg.solve(A, B)
print(f"الحل البرمجي (x, y): {X}") # المخرجات: [3. 2.]
