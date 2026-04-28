<font color="#95b3d7">**Spectral Theorem:** </font>Every symmetric matrix ${A \in \mathbb{R}^{n \times n}}$ has the following properties:
- All eigenvalues of A are real.
- Eigenvalues corresponding to distinct eigenvalues are orthogonal. 
- There exists an orthonormal basis for ${A\in\mathbb{R}^n}$ composed entirely of eigenvectors of A.
- A can be factored as ${}$
If a matrix is upper triangular, lower triangular or diagonal, the entries of the diagonals are equal to its eigenvalues. From there, you can directly read the eigenvalues.
- <font color="#95b3d7">**Orthogonal Diagonalization:**</font> The goal is to decompose the matrix in such a way that the transformation is reduced to pure scaling operation. The formula: $${A = QΛQ^T}$$Matrix multiplications take too much time, especially when we have something like ${A^{1000}v}$. In this case, we use orthogonal diagonalization to reduce computing time. Instead of computing ${A^{n}v}$, we compute ${A = (QΛQ^T)^{n}}$. Hence, we obtain ${(QΛQ^T).(QΛQ^T). ... v}$ and therefore $A^n = Q \Lambda^n Q^T$.