# shell scripting Project
## Assigning variable
name="John" 
echo $name
<img width="491" alt="1_assigning variable" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ea653a18-1216-4380-afb6-0ac873aebdb4">
## using if-else to execute script based on condition
#!/bin/bash
<img width="491" alt="2_using if else to execute script based on a condition" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/b1ee1668-4f4f-46a7-8c49-5a8912582112">
# Example script to check if a number is positive, negative, or zero
read -p "Enter a number: " num

if [ $num -gt 0 ]; then
    echo "The number is positive."
elif [ $num -lt 0 ]; then
    echo "The number is negative."
else
    echo "The number is zero."
fi

## iterating through a list using a forloop
!/bin/bash
# Example script to print numbers from 1 to 5 using a for loop

for (( i=1; i<=5; i++ ))
do
    echo $i
done
<img width="485" alt="3_Iterating through a list using a for loop" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/4a5df5d1-c507-4e8b-8b65-02eb9ed3ccba">


## using backtick and syntax for command substitution
current_date=`date +%Y-%m-%d`
<img width="486" alt="4_using backtick and syntax for command substitution" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c05a04ec-84f1-4d0a-86dd-b581a8433d64">

## input and output
echo "Enter your name:"
read name

echo "Hello World"

echo "hello world" > index.txt

grep "pattern" < input.txt

echo "hello world" | grep "pattern"
<img width="485" alt="5_input and output" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/4249c2b8-7fda-4311-a98b-f9120711dcfe">

## function
#!/bin/bash
# Define a function to greet the user
greet() {
    echo "Hello, $1! Nice to meet you."
}

# Call the greet function and pass the name as an argument
greet "John"
<img width="482" alt="6_function" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/db2cb32e-b29f-4a98-bde1-73cdc749e812">

## writing 1st script
mkdir shell-scripting
touch user-input.sh
<img width="487" alt="7_writing 1st shell script_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/26730c23-d361-41e9-8397-9ba33c36fb01">
<img width="489" alt="8_writing 1st shell script_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/56f109da-326b-49c1-87d7-166ebd77912b">

## Directory manipulation and navigation
open a file
touch navigating-linux-filesystem.sh

#!/bin/bash

# Display current directory
echo "Current directory: $PWD"

# Create a new directory
echo "Creating a new directory..."
mkdir my_directory
echo "New directory created."

# Change to the new directory
echo "Changing to the new directory..."
cd my_directory
echo "Current directory: $PWD"

# Create some files
echo "Creating files..."
touch file1.txt
touch file2.txt
echo "Files created."

# List the files in the current directory
echo "Files in the current directory:"
ls

# Move one level up
echo "Moving one level up..."
cd ..
echo "Current directory: $PWD"

# Remove the new directory and its contents
echo "Removing the new directory..."
rm -rf my_directory
echo "Directory removed."

# List the files in the current directory again
echo "Files in the current directory:"
ls

vi  navigating-linux-filesystem.sh

 sudo chmod +x navigating-linux-filesystem.sh

 ./navigating-linux-filesystem.sh
<img width="481" alt="9_directory manipulation_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/c4b08021-b00b-4ef2-9da7-8f679d6ac0b0">
<img width="487" alt="10_directory manipulation_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/0634173b-174c-4fff-a49a-e46b6f528486">

## file operation and sorting!!!!!
touch sorting.sh
vi sorting.sh
sudo chmod +x sorting.sh
<img width="484" alt="11_file operation and sorting_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/8fff0e04-9633-4c48-8244-e843fa35441b">
<img width="500" alt="12_file operation and sorting_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/d5bc9a9c-43ee-46b3-a951-2ef09761fbc6">
<img width="498" alt="13_file operation and sorting_3" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/358be81b-9502-469e-a1cc-13826535052d">

## working with numbers and calculations
create file
touch calculations.sh

paste below
#!/bin/bash

# Define two variables with numeric values
num1=10
num2=5

# Perform basic arithmetic operations
sum=$((num1 + num2))
difference=$((num1 - num2))
product=$((num1 * num2))
quotient=$((num1 / num2))
remainder=$((num1 % num2))

# Display the results
echo "Number 1: $num1"
echo "Number 2: $num2"
echo "Sum: $sum"
echo "Difference: $difference"
echo "Product: $product"
echo "Quotient: $quotient"
echo "Remainder: $remainder"

# Perform some more complex calculations
power_of_2=$((num1 ** 2))
square_root=$(echo "sqrt($num2)" | bc)

# Display the results
echo "Number 1 raised to the power of 2: $power_of_2"
echo "Square root of number 2: $square_root"

 vi calculations.sh

sudo chmod +x calculations.sh

./calculations.sh

<img width="484" alt="14_working with numbers and calculation_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a271f40c-5cee-4fd8-ab2b-e98f920354d1">
<img width="491" alt="15_working with numbers and calculation_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/6518ff8d-0d69-4ad8-a611-063dba02da35">
<img width="483" alt="16_working with numbers and calculation_3" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/e9690fab-fb8f-42d2-ad69-9abf4848c8c0">


## file backup and file stamping
tourch backup.sh

copy below

#!/bin/bash

# Define the source directory and backup directory
source_dir="/path/to/source_directory"
backup_dir="/path/to/backup_directory"

# Create a timestamp with the current date and time
timestamp=$(date +"%Y%m%d%H%M%S")

# Create a backup directory with the timestamp
backup_dir_with_timestamp="$backup_dir/backup_$timestamp"

# Create the backup directory
mkdir -p "$backup_dir_with_timestamp"

# Copy all files from the source directory to the backup directory
cp -r "$source_dir"/* "$backup_dir_with_timestamp"

# Display a message indicating the backup process is complete
echo "Backup completed. Files copied to: $backup_dir_with_timestamp"

vi backup.sh

<img width="487" alt="17_file backup and time stamping_1" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/ed42ffab-9cdb-47f4-9699-c00e127cf3a9">
<img width="499" alt="18_file backup and time stamping_2" src="https://github.com/jubrilala/DevOps_Ade/assets/88538653/a9a94fe4-e2d0-4c1f-9953-4279ab05fbf3">







