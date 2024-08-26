# Assignment 1: Symbolic Planning

In this assignment, you will use PDDL to formally define domains and problem descriptions for a planner. The goal of this exercise is to build familiarity with precise planning domain descriptions. There are three parts to this assignment, and each part will require you to create a domain definition file and a problem definition file for a total of six files (three domain definition files and three problem definition files).

**Important**: We will only work with the STRIPS subset of the PDDL 1.2 specifications for this assignment. The only requirement your PDDL files should include is `(:requirements :strips)` .

**Getting started**

For this assignment, you will use the [FastDownward](https://www.fast-downward.org/) planning system to design and test planning domains and problems. Note: version >= 23.06 compiles using the C++20 standard. The autograder runs version 22.12.

To get the planner and compile it:

1. Clone the code for fast-downward:
   ``` 
    git clone <https://github.com/aibasel/downward.git>  
    cd downward
    git checkout origin/release-22.12
   ```
3. Compile using the provided build script:  
    ```
    ./build.py
    ```

To run the planner and check the results:

1. Run the planner with a planner configuration (lama-first used here)  
    ```
    ./fast-downward.py --alias lama-first {domain-file.pddl} {problem-file.pddl}
    ``` 
   For example:
   ``` 
    ./fast-downward.py --alias lama-first cake-domain.pddl cake-problem.pddl
   ```
3. If a plan is found, it will be saved in the file sas_plan by default. You can inspect the plan in the file:  
    `cat sas_plan`

References:
- FastDownward project website: <https://www.fast-downward.org/>
- Repository for FastDownward: <https://github.com/aibasel/downward>
- FastDownward supported PDDL features list: <https://planning.wiki/ref/planners/fd>
- Environment setup Dependencies: <https://www.fast-downward.org/ObtainingAndRunningFastDownward>


## Part 1: Complex Eat-Cake

You have been provided a `cake-domain.pddl` and `cake-problem.pddl` for the `have-cake-eat-cake` problem discussed in class. Modify the domain and the problem to create a more complex setting:

1. Create file, `cake-complex-domain.pddl` with conntent from `cake-domain.pddl`. Modify the domain in `cake-complex-domain.pddl` so that once something is baked, the dishes are dirty, and need to be cleaned using the "clean" action. The resulting plan in the simple eat-cake-have-cake problem but using this complex domain should be:  
    ```
    (bake cake)  
    (eat cake)  
    (clean )  
    (bake cake)
    ```  
    **What to turn in**: `cake-complex-domain.pddl` , which should be compatible with the original provided cake-problem.pddl .  

2. Create file, `cake-complex-problem.pddl` with content from `cake-problem.pddl`. Expand the problem in `cake-complex-domain.pddl` to satisfy the additional objects pie and cookies, and to have a goal condition that includes eaten pie and eaten cookies.  
    The initial state must be empty.  
    The goal condition must be: `(and (eaten cake) (eaten pie) (eaten cookies)) `

    **What to turn in**: `cake-complex-problem.pddl`, which should be compatible with the original provided cake-domain.pddl .


## Part 2: Single-Piece King Moves

1. Create a PDDL domain definition, `king` in file `single-king-domain.pddl`, for a single king chess piece moving on an arbitrary sized board. You must use the following predicates, _and no others_:
- `(at ?p ?x ?y) `- Indicates that piece ?p is at the square ?x, ?y
- `(adjacent ?y1 ?y2)` - Indicates that the x- or y- coordinates ?y1, ?y2 are adjacent
- `(occupied ?x ?y)` - Indicates that the square ?x, ?y is occupied by another (untitled) piece
The domain definition must define a move action with the following signature:
  ```
  (:action move
        :parameters (?p ?x1 ?y1 ?x2 ?y2)
  ```
2. Create a compatible PDDL problem definition, `single-king-problem` in file `single-king-problem.pddl`, for an 8x7 (8 squares along y direction, and 7 along x direction) board, with the following initial state:
- The king starts out from the lower left corner of the board (1, 1).
- Mark the coordinates (7,7) and (6,7) as occupied.

The final condition should be for the king to reach (7, 8)
Your objects section must contain only the following:
```
(:objects
    king
    x1 x2 x3 x4 x5 x6 x7
    y1 y2 y3 y4 y5 y6 y7 y8
)
```
The domain definition must be precise enough to ensure that all valid moves are permissible and that all invalid moves are disallowed - we will test your domain definition for correctness.

**What to turn in**: The file `single-king-domain.pddl` , which defines the king's move, and the file `single-king-problem.pddl` that defines the above problem.


## Part 3: Single-Piece Knight Moves

Create a PDDL domain definition, `knight` in file `single-knight-domain.pddl`, for a single knight chess piece moving on an arbitrary sized board. You must use the following predicates, and _and no others_:

- `(at ?p ?x ?y)` - Indicates that piece ?p is at the square ?x, ?y
- `occupied ?x ?y)` - Indicates that the square ?x, ?y is occupied by another (untitled) piece
- `(delta1 ?y1 ?y2)`- Indicates that the x- or y- coordinates ?y1, ?y2 are adjacent
- `(delta2 ?y1 ?y2)` - Indicates that the x- or y- coordinates ?y1, ?y2 are separated by one square

The domain definition must define a move action with the following signature:
```
(:action move
    :parameters (?p ?x1 ?y1 ?x2 ?y2)
```
2. Create a compatible PDDL problem definition, `single-knight-problem` in file `single-knight-problem.pddl`, for an 8x7 (8 squares along y direction, and 7 along x direction) board, with the following initial state:
- The knight starts out from the lower left corner of the board (1, 1).
- Mark the coordinates (2,3) and (6,6) as occupied.
The final condition should be for the knight to reach (7, 8)
Your objects section must contain only the following:
```
(:objects
    knight
    x1 x2 x3 x4 x5 x6 x7
    y1 y2 y3 y4 y5 y6 y7 y8
)
```
The domain definition must be precise enough to ensure that all valid moves are permissible and that all invalid moves are disallowed - we will test your domain definition for correctness.

**What to turn in**: The file `single-knight-domain.pddl` , which defines the king's move, and the file `single-knight-problem.pddl` that defines the above problem.


## Extra Credit: Dual Knight Moves

Create a PDDL problem definition, `dual-knights-problem` in file `dual-knights-problem.pddl`, for an 8x7 (8 rows, 7 columns) board with two knight chess pieces. This problem definition must be compatible with the domain definition in part 3.
Your objects section must contain only the following:
```
(:objects
    knight1 knight2
    x1 x2 x3 x4 x5 x6 x7
    y1 y2 y3 y4 y5 y6 y7 y8
)
```
The initial state must have:
- knight1 at (1,1)
- knight2 at (7,8)
- Mark the coordinates (2,3) and (6,6) as occupied.
The final condition should be for knight1 and knight2 to swap positions.

**What to turn in**: The file `dual-knights-problem.pddl` , which must be compatible with your `single-knight-domain.pddl` from part 3.

# Submission Instructions
Please put files mentioned in "What to turn in" into zip file, and submit the zip file in Gradescope under AI388U-assignment1.
Your zip file should include the following files:
1. `cake-complex-domain.pddl` for part 1.1
2. `cake-complex-problem.pddl` for part 2.2
3. `single-king-domain.pddl` for part 2.1
4. `single-king-problem.pddl` for part 2.2
5. `single-knight-domain.pddl` for part 3.1
6. `single-knight-problem.pddl` for part 3.2
7. `dual-knights-problem.pddl` for Extra Credit
