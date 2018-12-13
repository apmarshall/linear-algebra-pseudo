# Definition of a Matrix

Ultimately, for our purposes, a matrix is simply an array of arrays, with each sub-array representing a row within the matrix.

So in code the matrix: 
| 1  0  0  0 |
| 0  1  0  0 |
| 0  0  1  0 |
| 0  0  0  1 |

Would be written as:
`[[1, 0, 0, 0],[0, 1, 0, 0],[0, 0, 1, 0],[0, 0, 0, 1]]`

Or, if we want to be sticklers about formatting, as: 
    [[1, 0, 0, 0],
     [0, 1, 0, 0],
     [0, 0, 1, 0],
     [0, 0, 0, 1]] 

That said, because any matrix may be arbitrarily sized, our data model needs to be self-referencing to accomodate such arbitrariness.

So our data definition for a matrix is as follows:

    Matrix is one of:
        - empty
        - Array + Matrix
    Interp: An array of arrays, each sub-array representing the row of a matrix

    (define M0 []) ; empty matrix 

    (define M1 [[1, 0, 0, 0],
                 [0, 1, 0, 0],
                 [0, 0, 1, 0],
                 [0, 0, 0, 1]]) ; 4x4 matrix

    (define M2 [[2]
                 [4]
                 [6]
                 [8]]) ; 1x4 Column Vector 

    (define M3 [[1, 2, 3, 4]]) ; 4x1 Row Vector

    (define (fn-for-matrix) 
        (if [empty? (...)]
            [else (... (first matrix))]
                (fn-for-matrix (rest matrix))))


