To be able to test/run the answer for Question #1
1. Make sure both numpy and pandas are imported.
2. Copy the following codes into a python file (with .py extension)

	import numpy as np
	import pandas as pd

	data = np.array([['X', 'X', 'O', 'O', 'X', 'O', 'X','O',  'X', 'O'], 
						   ['X', 'O', 'O', 'X', 'O', 'X', 'O', 'X', 'O', 'X'], 
						   ['O', 'X', 'X', '',  'X', 'O', 'O', 'X', 'O', 'X'], 
						   ['O', 'X', 'X', 'X', 'O', 'O', 'X', '',  'X', 'O'],
						   ['X', 'O', 'O', 'X', 'O', 'X', 'O', 'X', 'X', 'O'],
						   ['X', 'O', 'X', '',  'X', 'X', 'O', '',  'O', 'X'],
						   ['X', 'X', 'O', 'O', 'X', 'O', 'X', 'O', 'X', 'O'],
						   ['X', 'O', '',  'O', 'X', 'O', 'O', 'X', 'O', 'O'],
						   ['O', 'X', 'X', 'X', 'O', 'O', '',  'O', 'X', 'O'],
						   ['O', 'O', 'O', 'O', 'O', 'O', 'O', 'X', 'O', 'X']
						  ]
						 )
						 
	grid = pd.DataFrame(data)
	print(grid)
	count_x = np.count_nonzero(data == 'X')
	count_o = np.count_nonzero(data == 'O')

3. From the CLI, run the .py file by invoking the command below:
   python <name of .py file> 
   
4. Check that the values of the 2D array are correct, based on the code.


To be able to test/run the answer for Question #2
1. Make sure you run first the codes for the answer for Question #1 
2. Copy the following codes into a python file (with .py extension)

	def create_smaller_2D_array(grid):
	   row_start = int(input('Enter row start pos:'))
	   row_end = int(input('Enter row end pos:'))
	   col_start = int(input('Enter column start pos:'))
	   col_end = int(input('Enter column end pos:'))
	   new_smaller_2D_array = grid.loc[row_start:row_end, col_start:col_end]
	   return new_smaller_2D_array

	new_smaller_2D_array = create_smaller_2D_array(grid)
	print(new_smaller_2D_array)
	
3. From the CLI, run the .py file by invoking the command below:
   python <name of .py file> 

4. A new smaller 2D array should have been created.


To be able to run the answer for Question #3
1. Make sure you run first the codes for the answer for Question #1
2. Copy the following codes into a python file (with .py extension)

	from random import uniform, seed
	from math import sqrt
	seed(5) # to produce random numbers

	class Agent:
		def __init__(self, type):
			self.type = type
			self.draw_location()

		def draw_location(self):
			self.location = uniform(0, 1), uniform(0, 1)

		def get_distance(self, other):
			"Computes the euclidean distance between self and other agent."
			a = (self.location[0] - other.location[0])**2
			b = (self.location[1] - other.location[1])**2
			return sqrt(a + b)


		# function that checks the threshold, and calculates the satisfied characters
		def calculate_satisfied_neighbors(self, agents):
			# Return True if sufficient number of nearest neighbors are of the same type.
			
			distances = []
			# distances is a list of pairs (d, agent), where d is distance from agent to self
			for agent in agents:
				if self != agent:
					distance = self.get_distance(agent)
					distances.append((distance, agent))
					
			# Sort from smallest to largest, according to distance 
			distances.sort()
			
			# Get the satisfied neighboring characters 
			neighbors = [agent for d, agent in distances[:num_neighbors]]
			satisfied_neighbors = [agent for agent in neighbors]
			
			# Generate a new 2D array (2,5), for the satisfied neighboring characters
			satisfied_neighboring_characters = np.array(satisfied_neighbors).reshape(2,5)
			print(f'satisfied_neighboring_characters: {satisfied_neighboring_characters}')
			
			# Count how many neighbors have the same type
			num_same_type = sum(self.type == agent.type for agent in neighbors)
			return num_same_type >= require_same_type
			
		
			
		def update(self, agents):
			# If agent is not happy, then choose new locations randomly by calling draw_location, until agent is happy.
			while not self.calculate_satisfied_neighbors(agents):
				self.draw_location()


	num_of_type_X = count_x
	num_of_type_O = count_o

	# the number of agents that are regarded as neighbors
	num_neighbors = 10  
	# threshold value to say that at least this many neighbors to be same type, for the agent to be happy    
	require_same_type = 5   

	# Create a list of agents
	agents = [Agent('X') for i in range(num_of_type_X)]
	agents.extend(Agent('O') for i in range(num_of_type_O))



	"""while agents are not happy (means that agent want to move)
		for agent in agents
			give agent the opportunity to move
	"""
	count = 1
	# Loop until none of the agents wants to move
	while True:
		print('Entering loop ', count)
		count += 1
		no_one_moved = True
		for agent in agents:
			old_location = agent.location
			agent.update(agents)
			if agent.location != old_location:
				no_one_moved = False
		if no_one_moved:
			break
	print('Exiting.')
	
3. From the CLI, run the .py file by invoking the command below:
   python <name of .py file>

4. To check, you should see something like the one shown below as an output:

   satisfied_neighboring_characters: [[<__main__.Agent object at 0x000002460B5D8D08>
  <__main__.Agent object at 0x000002460B5D8148>
  <__main__.Agent object at 0x000002460B5D1D88>
  <__main__.Agent object at 0x000002460B5D1B48>
  <__main__.Agent object at 0x000002460B5D11C8>]
 [<__main__.Agent object at 0x000002460B5D8D88>
  <__main__.Agent object at 0x000002460B5D1648>
  <__main__.Agent object at 0x000002460B5D1548>
  <__main__.Agent object at 0x000002460B5D8888>
  <__main__.Agent object at 0x000002460B5D8748>]]
  Exiting.


