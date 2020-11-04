# Matrix Multiplication

"Every Row dot Every Column"
"Every Row (of the left hand side) dot (product) Every Column (of the right hand side)"

Row vs Column Major:  Memory Layout<br>
Row vs Column *Vectors*:  Math order (`row * mat` or `mat * col`)<br>
D3D vs OpenGL/Mathematicians<br>
https://www.mindcontrol.org/~hplus/graphics/matrix-layout.html<br>

https://github.com/JaapSuter/Ten18/blob/master/lib/DirectXTex/XNAMath/xnamath.h#L2742<br>
https://github.com/microsoft/DirectXMath/blob/7fcdfbc3c64d5c695b39fb376c1f5fb52e1084db/Inc/DirectXMathMatrix.inl#L1964<br>
https://github.com/microsoft/DirectXMath/blob/7fcdfbc3c64d5c695b39fb376c1f5fb52e1084db/Inc/DirectXMathMatrix.inl#L2002<br>

|                 |     |
| --------------- | --- |
| Associative     | (AB)C = A(BC)
| Distirubtive    | A(B+C) = AB+AC
| NOT commutative | (AB != BA)
