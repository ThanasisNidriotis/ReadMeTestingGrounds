# Aliquot Sequence Program

Welcome to the README for the Aliquot Sequence Program.

This program was created for the execution of the mathematical sequence called the [Aliquot Sequence](https://en.wikipedia.org/wiki/Aliquot_sequence).

# 1. What is the Aliquot Sequence?
The Aliquot Sequence,in mathematics, is a sequence of positive integers in which each term is the sum of the [proper divisors](https://en.wikipedia.org/wiki/Divisor#proper_divisor) of the previous term. The sequence runs until it reaches the number 1, where it ends since the sum of the proper divisors of 1 is 0.

For example: If you choose the number 9 to be the starting point the sequence goes as follows  
```
    Proper factors of 9 are : 1,3 the sum of which is 4
    Proper factors of 4 are : 1,2 the sum of which is 3
    Proper factor of 3 is : 1 
    And the sum of the proper factors of 1 is 0 
    So the sequence looks like this : 9->4->3->1
```
However the aliquot sesquence does not always terminate.There are different categories of numbers for which this is true.

Specifically:

- **Perfect Numbers** have a repeating aliquot sequence period of 1.For example the number 6 has an aliquot sequence of 6,6,6,6,6,6.. and so on.

 - **Amicable Numbers** work in pears and have a repeating aliquot sequence period of 2.Such numbers are 220 and 284 , the sum of the proper divisors of the former give us the latter and vice versa.
 
 - **Social Numbers** are like Amicable ones,with the key difference being that they have a period of 3 or greater.

 - **Aspiring Numbers**  are certain numbers that have an aliquot sequence which is eventually periodic but the numbers do not belong to any of the previous categories.For instance the number 95 has an aliquot sequence of :95,25,6,6,6,6...

 Lastly there are certain numbers like the infamous *276* for whose aliquot sequence we do not know whether it terminates or ends in a repeating cycle.

 # 2. What does this program accomplish?

This program has as its main function the calculation of the Aliquot Sequence of a specific number which is requested by the user to input.The use can also choose some other options for the program to run.

Specifically:
 - The user is requested to input a positive integer number smaller than 10^15
 - The user is requested for the maximum length of the aliquot sequence they want the program to print.For instance if the user wants to get first 3 numbers for the aliquote sequence of 12 the program would print, besides the original number of the sequence,16,15,9 .In case the user inputs 0, the sequence runs either until it reaches 1 and it terminates, or for an unlimted length (in case of a perfect, amicable or social number, the sequence runs until the number exceeds 10^15 or the program is forced to terminate).
 - Lastly the user is asked to input the character 'f' for the **full length** of the sequence or the character 'l' for just the **length** of the sequence. 

 ### How does this program accomplish the calculation of the Aliquot sequence?
``` c
    The program uses the following function:

    long long int ali(long long int aliquot_number){
	long long int step; //the Step of the loop//
	long long int temp = 0; //The sum of the perfect divisors of the number//
	for (step = 1; step*step < aliquot_number ; step++ ){
		if (aliquot_number % step == 0){  // Checks if the current step is a factor of the number//
			temp = temp + step + aliquot_number/step; 
		}	
    }
	if (step*step == aliquot_number){
		temp = temp + step;
	}
	temp = temp -aliquot_number; 
	return temp ;
}
```
1. The algorithm takes the number the user inputs and store it in the variable **aliquot_number**.

2. Afterwards using the for(..) loop, it checks positive integers starting from 1 until it reaches the factor that is equal to or surpasses the root of the aliquot number.
The reason this was choosen is because according to mathematics a [composite number](https://en.wikipedia.org/wiki/Composite_number) cant have a a divisor which is greater than it's root.This way regarding very large numbers we eliminate unnecessary calculations that may hinder the efficiency and speed of the program.

3. The algorithm then checks for each number whether it is a proper divisor of the aliquot_number and then proceeds to add to the sum **the divisor being tested and the result of the divison of the aliquot_number with the divisor**.This is done because the the result of the division is also a divisor of the aliquot_number.(For example the number 12 has as a divisor the number 2. The result of the division of the former with the latter is the number 6 which is also a factor of 12).By doing this in addition to using the root method for calculating the factors, we add any possible factors the algorithm would miss otherwise.

4. Lastly the function checks whether the factor that breaks the loop is equal to the root of the aliquot_number and is added only a single time (because 
otherwise we would add the save value twice since aliquot_number/step = step ) and it also removes from the sum the aliquot_number because we start the loop from 1 and as such we add the number to the sum from the aliquot_number/step equation.

## :arrow_down: How to compile and execute this program

To compile this program,after downloading the file,you need to compile it using the gcc compiler and inputing this line of code in the terminal of your device:
```
 gcc -O0 -m32 -Wall -Wextra -Werror -pedantic -o aliquot 
 aliquot.c
```
This way you may execute the program by typing the command
```
./aliquot
```


# 3.:warning: Test runs, expected results and errors.

This program ,like the previous chapters mention, calculates the aliquot sequence, according to a specific number the user inputs in combination with the inputs regarding the length of the sequence and the whether the user wants the full sequence or just the length.

### Expected inputs:
This program expects the user to input a specific type of character after being promted to do so:
1. The first input has to be a positive integer from 1 to 10^15(or 1000000000000000).

2. The second input needs to be a non negative integer.

3. The last input needs to be either the character **'f'** or the character **'l'**

##### Example: If the user inputs the number 12 as the number to start the sequence from, the length being unlimited (input being 0) and printing the full length.[![asciicast](https://asciinema.org/a/WejpFXSRzpkZbD0lMzzD6Tpbh.svg)](https://asciinema.org/a/WejpFXSRzpkZbD0lMzzD6Tpbh)

In the scenario where the user inputs something that is outside the parameters given by the program, the execution stops and the program returns an exit code of 1

#### Example:If the user inputs a number greater than 10^15 or inputs an alphabetical character for the first 2 inputs. [![asciicast](https://asciinema.org/a/pIsWN60t5tnHnmRH6G68r5Tok.svg)](https://asciinema.org/a/pIsWN60t5tnHnmRH6G68r5Tok)  

Lastly, in the case where the user inputs a number like 276, when the sequence reaches a number that exceeds 10^15 it also stops and returns an exit code of 1.

#### Important note
In this program i use the header library called **stdio.h** as it allows me to use *printf* function to print my results and *scanf* function so the user can input their desired numbers and characters.
The unfortunate thing is that due to the nature of *scanf* the user ought to be wary of what they input because, if for example they input the alphanumeric character 22f, the program will do the following:[![asciicast](https://asciinema.org/a/ST2bH30cv19xlxVs2rw8fwh9l.svg)](https://asciinema.org/a/ST2bH30cv19xlxVs2rw8fwh9l).
As you can see the program stops but bugs out, I havent found a way for this to not happen so I Leave this as a note to the user

I hope you find this program useful:grinning:!

----------------------------------------------------
#### Some Sources: 
[Aliquot number](https://share.google/9UYIIg9QBtyaYaeFp)
[A youtube video which helped me understand the sequence](https://www.youtube.com/watch?v=OtYKDzXwDEE&t=3s)
