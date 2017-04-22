# Artificial Intelligence Nanodegree
## Introductory Project: Diagonal Sudoku Solver

# Question 1 (Naked Twins)
Q: How do we use constraint propagation to solve the naked twins problem?  
A: In constraing propagation method, we set a constraint and try to solve the suduko by drawing up different possibilites that satisfy the set constraint (for example, using a depth first search approach). 
If a solution is not identified for the set constraint, we expand constraint and again try solving the sudoko.

In Naked Twins strategy, we first identify boxes of size 2. 

	# list all the twin boxes
	twin_boxes = [box for box in values.keys() if len(values[box]) == 2]

Some of these are potential naked twins. We then iterate over these boxes and for each box, we come up with the peer list.
If the values in any of the peer boxes is matching, we add the peer box, the unit and the values to a new structure called twins. 

    for box in twin_boxes:
        for peer in peers[box]:
            if (values[box] == values[peer]):
                unit = [unit for unit in units[box] if peer in unit]
                #Build a dictionary for twins (values, pairs and units)
                twins.append({"value":values[box], "naked_twins":[box, peer], "units":unit})

Our twins structure is a dictionary with the two-digit value, the naked_twins (boxes) and the list of units to which they belong.
Now we iterate through this structure and for each entry, we identify the non-twin peers (other boxes) in list of common units.
For each of these non-twin peers, we remove the digits that are present in the naked_twins.

    # Now iterate through the twins structure and remove the digits on non-twin boxes in a unit
    for twin_box in twins:
        for unit in twin_box["units"]:
            for dig in twin_box["value"]:
                other_boxes = [box for box in unit \
                              if box not in twin_box["naked_twins"] and dig in values[box]]
                for peer_box in other_boxes:
                    assign_value(values, peer_box, values[peer_box].replace(dig,''))

The returned values will solve the sudoku using the naked_twins strategy.


# Question 2 (Diagonal Sudoku)
Q: How do we use constraint propagation to solve the diagonal sudoku problem?  
A: We use the same Naked Twins strategy, but expand the unitlist to include the two diagonal set of boxes as follows:
The reverse columns (revcols) is a list in reverse (987654321).

	diag1_units = [[rows[i]+cols[i] for i in range(len(rows))]]
	diag2_units = [[rows[i]+revcols[i] for i in range(len(rows))]]
	unitlist = row_units + column_units + square_units + diag1_units + diag2_units

We use the same naked twins strategy. In our twins structure, the "units" may also include the two diagonal units.
The search for peers will then expand in the diagonal units as well.

### Install

This project requires **Python 3**.

We recommend students install [Anaconda](https://www.continuum.io/downloads), a pre-packaged Python distribution that contains all of the necessary libraries and software for this project. 
Please try using the environment we provided in the Anaconda lesson of the Nanodegree.

##### Optional: Pygame

Optionally, you can also install pygame if you want to see your visualization. If you've followed our instructions for setting up our conda environment, you should be all set.

If not, please see how to download pygame [here](http://www.pygame.org/download.shtml).

### Code

* `solution.py` - You'll fill this in as part of your solution.
* `solution_test.py` - Do not modify this. You can test your solution by running `python solution_test.py`.
* `PySudoku.py` - Do not modify this. This is code for visualizing your solution.
* `visualize.py` - Do not modify this. This is code for visualizing your solution.

### Visualizing

To visualize your solution, please only assign values to the values_dict using the ```assign_values``` function provided in solution.py

### Submission
Before submitting your solution to a reviewer, you are required to submit your project to Udacity's Project Assistant, which will provide some initial feedback.  

The setup is simple.  If you have not installed the client tool already, then you may do so with the command `pip install udacity-pa`.  

To submit your code to the project assistant, run `udacity submit` from within the top-level directory of this project.  You will be prompted for a username and password.  If you login using google or facebook, visit [this link](https://project-assistant.udacity.com/auth_tokens/jwt_login for alternate login instructions.

This process will create a zipfile in your top-level directory named sudoku-<id>.zip.  This is the file that you should submit to the Udacity reviews system.

