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

That said, because any matrix may be arbitrarily sized, our data model needs to be self-referencing to accommodate such arbitrariness. This means it will need both an "empty" case and a compound case. This might yield something like this:

    Matrix is one of:
        - [] (an empty array)
        - Array + Matrix
    Interp: An array of arrays, each sub-array representing the row of a matrix

    (define M0 []) ; empty matrix 

    (define M1 [[1, 0, 0, 0],
                [0, 1, 0, 0],
                [0, 0, 1, 0],
                [0, 0, 0, 1]
                []]) ; 4x4 identity matrix

    (define M2 [[2],
                [4],
                [6],
                [8],
                []]) ; 1x4 Column Vector 

    (define M3 [[1, 2, 3, 4],[]]) ; 4x1 Row Vector

    (define fn-for-matrix
        if empty? (...)
            else (... (first matrix))
                fn-for-matrix (rest matrix))


One consideration is that we need to know when we've reached the "base" condition (empty). For this reason, each matrix needs to end with an empty element, which will trigger the base case in any functions using this data model.

Another consideration is that in a matrix, every row must have the same number of items (minus the final empty row). We'll need to develop tests for this that determine that a matrix is properly formatted:

    Input -> Boolean
    Determines whether the input qualifies as a matrix.
    (check-expect is-true-matrix? M0 true)
    (check-expect is-true-matrix? M1 true)
    (check-expect is-true-matrix? M2 true)
    (check-expect is-true-matrix? M3 true)
    (check-expect is-true-matrix? [[1,2,3],[2,4,6,8],[]] false)

    (define (is-true-matrix? input)
        if (empty? || if (next matrix) empty?) (return true)
            else (if elements.count (first matrix) != elements.count (next matrix)) (return false)
                    else is-true-matrix? (rest matrix))

Finally we are going to want to be able to identify some particular types of matrices (namely, row and column vectors):

    Matrix -> Boolean
    Determines whether a matrix is a column vector
    (check-expect is-column-vector? M0 false)
    (check-expect is-column-vector? M1 false)
    (check-expect is-column-vector? M2 true)
    (check-expect is-column-vector? M3 false)

    (define (is-column-vector? matrix) 
        if (is-true-matrix? false) (return "Error: Not a Matrix")
            else (if empty?) (...)
                else (... (first matrix))
                    is-column-vector? (rest matrix))

    Matrix -> Boolean
    Determines whether a matrix is a row vector
    (define (is-row-vector? matrix) false)
    (check-expect is-row-vector? M0 false)
    (check-expect is-row-vector? M1 false)
    (check-expect is-row-vector? M2 false)
    (check-expect is-row-vector? M3 true)

    (define (is-row-vector? matrix) 
        if (is-true-matrix? false) (return "Error: Not a Matrix")
            else (if empty?) (...)
                else (... (first matrix))
                    is-row-vector? (rest matrix))
