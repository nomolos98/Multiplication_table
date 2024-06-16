
# Capstone Project -Linux Shell Scripting

## Multiplication Timetable Generator

This Bash script generates a multiplication table for a number entered by the user. The user can choose between generating a full table (from 1 to 10) or a partial table within a specified range. The script ensures valid input and provides clear and readable output.

## Usage

1. Log into my ubuntu server
2. Create multiplication_table.sh by running the command vim multiplication_table.sh
3. You can make the script executable at this stage
    ```bash
    chmod +x multiplication_table.sh
    ```
4. To run the script, you can either do the ./ or type bash
    ```bash
    ./multiplication_table.sh or bash multiplication_table.sh
    ```
5. Start your shell scripting by typing shebang "#! /bin/bash"

## Script Breakdown

### 1. Prompting User Input
The script starts by asking the user to enter a number for which the multiplication table will be generated.
```bash
read -p "Enter a number for the multiplication table: " number
```
### 2. Validate the input number 
The input is validated to ensure it is a valid integer. If not, the script exits with an error message
```bash
if ! [[ "$number" =~ ^[0-9]+$ ]]; then 
echo "Invalid number. Please enter a valid integer." 
exit 1 
fi
```
### 3. Full or Partial Table Choice
The user is asked whether they want a full table or a partial table. This choice is stored in the variable "choice"
```bash
read -p "Do you want a full table or a partial table? (Enter 'f' for full, 'p' for partial): " choice
```
- **Full Table**: If the user chooses a full table (`f`), the script generates a multiplication table from 1 to 10.
-   **Partial Table**: If the user chooses a partial table (`p`), the script prompts for the starting and ending numbers of the range. These inputs are validated to ensure they are within 1 to 10 and the start is less than or equal to the end
```bash
if [ "$choice" == "f" ]; then
    echo "The full multiplication table for $number:"
    generate_table $number 1 10
elif [ "$choice" == "p" ]; then
    read -p "Enter the starting number (between 1 and 10): " start
    read -p "Enter the ending number (between 1 and 10): " end
    
    if ! [[ "$start" =~ ^[0-9]+$ ]] || ! [[ "$end" =~ ^[0-9]+$ ]] || [ $start -lt 1 ] || [ $start -gt 10 ] || [ $end -lt 1 ] || [ $end -gt 10 ] || [ $start -gt $end ]; then
        echo "Invalid range. Showing full table instead."
        generate_table $number 1 10
    else
        echo "The partial multiplication table for $number from $start to $end:"
        generate_table $number $start $end
    fi
else
    echo "Invalid choice. Showing full table instead."
    generate_table $number 1 10
fi
```
### 4. We create the Multiplication Table function

The `generate_table` function generates the multiplication table based on the provided range. It uses a for loop to iterate through the range and print the multiplication results.
```bash
generate_table() {
    local number=$1
    local start=$2
    local end=$3

    for ((i=start; i<=end; i++)); do
        echo "$number x $i = $((number * i))"
    done
}
```
### 5. Addition: Creating Ascending or Descending Order

The script asks the user if they want to see the table in ascending or descending order. Based on the input (`a` or `d`), the table is displayed accordingly.
```bash
read -p "Do you want the table in ascending or descending order? (Enter 'a' for ascending, 'd' for descending): " order

if [ "$order" == "a" ]; then
    generate_table $number 1 10
elif [ "$order" == "d" ]; then
    for ((i=10; i>=1; i--)); do
        echo "$number x $i = $((number * i))"
    done
else
    echo "Invalid choice. Showing table in ascending order."
    generate_table $number 1 10
fi
```
### 6. User Interaction feature

After displaying the table, the script asks if the user wants to generate another table. If the user chooses `y`, the script restarts by executing itself (`exec $0`). If the user chooses `n`, the script exits.
```bash
while true; do
    read -p "Do you want to generate another table? (y/n): " repeat
    if [ "$repeat" == "y" ]; then
        exec $0
    else
        echo "Goodbye!"
        exit 0
    fi
done
```
#### Summary of the complete code
```bash
#!/bin/bash
# Function to generate multiplication table
generate_table() {
    local number=$1
    local start=$2
    local end=$3

    for ((i=start; i<=end; i++)); do
        echo "$number x $i = $((number * i))"
    done
}

# Prompt the user to enter a number
read -p "Enter a number for the multiplication table: " number

# Validate the input number
if ! [[ "$number" =~ ^[0-9]+$ ]]; then
    echo "Invalid number. Please enter a valid integer."
    exit 1
fi

# Ask the user if they want a full table or a partial table
read -p "Do you want a full table or a partial table? (Enter 'f' for full, 'p' for partial): " choice

# Handle the user's choice
if [ "$choice" == "f" ]; then
    echo "The full multiplication table for $number:"
    generate_table $number 1 10
elif [ "$choice" == "p" ]; then
    # Prompt for the range
    read -p "Enter the starting number (between 1 and 10): " start
    read -p "Enter the ending number (between 1 and 10): " end
    
    # Validate the range
    if ! [[ "$start" =~ ^[0-9]+$ ]] || ! [[ "$end" =~ ^[0-9]+$ ]] || [ $start -lt 1 ] || [ $start -gt 10 ] || [ $end -lt 1 ] || [ $end -gt 10 ] || [ $start -gt $end ]; then
        echo "Invalid range. Showing full table instead."
        generate_table $number 1 10
    else
        echo "The partial multiplication table for $number from $start to $end:"
        generate_table $number $start $end
    fi
else
    echo "Invalid choice. Showing full table instead."
    generate_table $number 1 10
fi

# Bonus: Ask the user if they want to see the table in ascending or descending order
read -p "Do you want to see the table in ascending or descending order? (Enter 'a' for ascending, 'd' for descending): " order

# Handle the order choice
if [ "$order" == "a" ]; then
    generate_table $number 1 10
elif [ "$order" == "d" ]; then
    for ((i=10; i>=1; i--)); do
        echo "$number x $i = $((number * i))"
    done
else
    echo "Invalid choice. Showing table in ascending order."
    generate_table $number 1 10
fi

# Enhanced User Interaction: Repeat the program for another number without restarting the script
while true; do
    read -p "Do you want to generate another table? (y/n): " repeat
    if [ "$repeat" == "y" ]; then
        exec $0
    else
        echo "Goodbye!"
        exit 0
    fi
done
```
### Sample Output
##### Full Table
```bash
Enter a number for the multiplication table: 3
Do you want a full table or a partial table? (Enter 'f' for full, 'p' for partial): f
The full multiplication table for 3:
3 x 1 = 3
3 x 2 = 6
3 x 3 = 9
3 x 4 = 12
3 x 5 = 15
3 x 6 = 18
3 x 7 = 21
3 x 8 = 24
3 x 9 = 27
3 x 10 = 30
```
##### Partial Table
```bash
Enter a number for the multiplication table: 3
Do you want a full table or a partial table? (Enter 'f' for full, 'p' for partial): p
Enter the starting number (between 1 and 10): 2
Enter the ending number (between 1 and 10): 4
The partial multiplication table for 3 from 2 to 4:
3 x 2 = 6
3 x 3 = 9
3 x 4 = 12
```
##### Invalid range validation
```bash
Enter a number for the multiplication table: 3
Do you want a full table or a partial table? (Enter 'f' for full, 'p' for partial): p
Enter the starting number (between 1 and 10): 8
Enter the ending number (between 1 and 10): 2
Invalid range. Showing full table instead.
The full multiplication table for 3:
3 x 1 = 3
3 x 2 = 6
3 x 3 = 9
3 x 4 = 12
3 x 5 = 15
3 x 6 = 18
3 x 7 = 21
3 x 8 = 24
3 x 9 = 27
3 x 10 = 30
```


