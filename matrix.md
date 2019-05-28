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
                [0, 0, 0, 1]]) ; 4x4 identity matrix

    (define M2 [[2],
                [4],
                [6],
                [8]]) ; 1x4 Column Vector 

    (define M3 [[1, 2, 3, 4]]) ; 4x1 Row Vector

    (define fn-for-matrix
        if empty? (...)
        else (... (first matrix))
             fn-for-matrix (rest matrix))


One consideration is that we need to know when we have reached the "empty" or base condition. Ultimately, in the examples we have given this occurs when there are no more elements in the outer matrix (ie, the parent matrix is empty). The alternative is to close out each matrix explicitly with an "empty" matrix as its last element. Either way, we will need to be building in a way to check for this within our loops.

Another consideration is that in a matrix, every row must have the same number of items. We'll need to develop tests for this that determine that a matrix is properly formatted:

    Input -> Boolean
    Determines whether the input qualifies as a matrix.
    (check-expect is-true-matrix? M0 true)
    (check-expect is-true-matrix? M1 true)
    (check-expect is-true-matrix? M2 true)
    (check-expect is-true-matrix? M3 true)
    (check-expect is-true-matrix? [[1,2,3],[2,4,6,8]] false)

    (define (is-true-matrix? input)
        if (empty? (return true)
            else (if (elements.count (first matrix)) != (elements.count (next matrix))) (return false)
                  else is-true-matrix? (rest matrix)))

Finally we are going to want to be able to identify some particular types of matrices (namely, row and column vectors, and square and identity matrices):

For a column vector, the check is simply to determine if each sub-array contains only one element:

    Matrix -> Boolean
    Determines whether a matrix is a column vector
    (check-expect is-column-vector? M0 false)
    (check-expect is-column-vector? M1 false)
    (check-expect is-column-vector? M2 true)
    (check-expect is-column-vector? M3 false)

    (define (is-column-vector? matrix) 
        if (is-true-matrix? false) (return "Error: Not a Matrix")
        else (if empty?) (return false)
              else (if (array.length = 1) (first matrix)) (return true)
                    else (return false))

For a row vector, we will want to determine if there is only one sub-array:

    Matrix -> Boolean
    Determines whether a matrix is a row vector
    (define (is-row-vector? matrix) false)
    (check-expect is-row-vector? M0 false)
    (check-expect is-row-vector? M1 false)
    (check-expect is-row-vector? M2 false)
    (check-expect is-row-vector? M3 true)

    (define (is-row-vector? matrix) 
        if (is-true-matrix? false) (return "Error: Not a Matrix")
        else (if empty?) (return false)
              else (if (array.length = 1) matrix) (return true)
                    else (return false))

So there's an important but subtle difference between the approaches we are taking in these two tests:

In the column vector test, we are evaluating the first element of what we have already determined to be a true matrix. If it is only one element long, then we are done, it is a column vecotr. In every other case, the answer is false.

In the row vector test, we are evaluating the entire matrix as a whole (again, after determining that is a true matrix) and testing whether it contains only one sub-element. If this is true, then we have a row vector, otherwise we do not.

Again, the column vector test is examining the element inside the matrix, the row vector test is examining the matrix as a whole.

To identify a square matrix, we will want to determine if the number of elements in each row is equal to the number of elements in each column. Assuming we have already verified that our array of arrays is a true matrix, this can easily be accomplished by comparing the number of elements in the first sub-array with the total number of sub-arrays.

    Matrix -> Boolean
    Determines whether a matrix is square or not
    (define (is-square-matrix? matrix) false)
    (check-expect is-square-matrix? M0 false)
    (check-expect is-square-matrix? M1 true)
    (check-expect is-square-matrix? M2 false)
    (check-expect is square-matrix? M3 false)
    
    (define (is-square-matrix? matrix)
        if (is-true-matrix? false) (return "Error: Not a Matrix")
        else (if empty?) (return false)
              else (if (array.length (first matrix)) = (array.length matrix)) (return true)
                    else (return false))


Finally, determining of a matrix is an identity matrix will be the most complicated test yet: it will require us to keep track of which row we are in so we can determine which column should be checked.
