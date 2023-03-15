# Shivakumar
You are given two input sheets and two output sheets and you are required to write a python script that reads the input fields and creates the same output as given.
The rules of leaderboards (output sheets) are mentioned in the corresponding sub-sheets.
import pandas as pd

# Step 1: Read all the sub-sheets
input_sheet1 = pd.read_excel('input_file.xlsx', sheet_name='input_sheet1')
input_sheet2 = pd.read_excel('input_file.xlsx', sheet_name='input_sheet2')

output_sheet1 = pd.read_excel('output_file.xlsx', sheet_name='output_sheet1')
output_sheet2 = pd.read_excel('output_file.xlsx', sheet_name='output_sheet2')

# Step 2: Make a note of all the input fields and output fields
# Note: The field names and data types are just examples and should be updated as per the actual input and output sheets
input_fields1 = ['name', 'score']
input_fields2 = ['id', 'team', 'score']
output_fields1 = ['rank', 'name', 'score']
output_fields2 = ['rank', 'id', 'team', 'score']

# Step 3: Write a Python Script that produces the same outputs
# For the sake of simplicity, let's assume that the rules for leaderboards are as follows:
# For output_sheet1: Rank the players by their score in descending order
# For output_sheet2: Rank the teams by their total score in descending order, and then by the team ID in ascending order

# Output Sheet 1
output_sheet1['rank'] = output_sheet1['score'].rank(method='dense', ascending=False)
output_sheet1 = output_sheet1.sort_values(by=['rank'])

# Select only the required fields
output_sheet1 = output_sheet1[input_fields1 + ['rank']]

# Output Sheet 2
# Group the input by team ID and calculate the total score for each team
grouped_input_sheet2 = input_sheet2.groupby('team').agg({'score': 'sum', 'id': 'min'})

# Rank the teams by their total score in descending order, and then by the team ID in ascending order
grouped_input_sheet2['rank'] = grouped_input_sheet2['score'].rank(method='dense', ascending=False)
grouped_input_sheet2 = grouped_input_sheet2.sort_values(by=['rank', 'id'])

# Select only the required fields
output_sheet2 = grouped_input_sheet2[input_fields2 + ['rank']]

# Write the output to a new file
with pd.ExcelWriter('output_file_generated.xlsx') as writer:
    output_sheet1.to_excel(writer, sheet_name='output_sheet1', index=False)
    output_sheet2.to_excel(writer, sheet_name='output_sheet2', index=False)
