# The Soundwave program
Welcome to the ReadMe for the Soundwave program!

This program was created in order to do a variety of functions and edit and utilize [wav](https://en.wikipedia.org/wiki/WAV) files in a number of different ways.

### Table of contents
- [1. What is a Wav/WAVE file?](#questionwhat-is-a-wavwave-file)
- [2.What does this program accomplish?](#what-does-this-program-accomplish)
    - [:clipboard:Info](#clipboard-info)
    - [:timer_clock:Rate](#timer_clock-rate)
    - [:train:Channel](#train-channel)
    - [:loud_sound:Volume](#loud_sound-volume)
    - [:bulb:Generate](#bulbgenerate)
- [:mag:3.Libraries used, Compiler and How to run!](#mag-3--libraries-used-compiler-and-how-to-run)
- [Sources](#sources)

--------------------------------------------------------
## :question: 1. What is a Wav/WAVE file?
A Wav file , is an audio file format for storing an audio bitstream.It is the main format used by Microsoft Windows systems for uncompressed audio.It consists of 3 parts
1. The FileHeader which is composed of 44 bytes of information like: The size of the file, whether its Mono or Stereo and other useful information.
2. The Sample Data which is the actual sound data that when played in the specific order,is relayed as "music", the amount of samples is found in the FileHeader , in the data chunk section.
3. The Other Data, which is as the name implies, other data which for the purpose of this program dont have any functional use.
---------------------------------------------------------
##  2. What does this program accomplish?

This program consists of 5 subprograms or functions which do different things depending on what the use inputs.
Specifically there is :
#### :clipboard: Info
-  The **Info** subprogram, which receives as input a ***.wav*** file and prints out the contets of the FileHeader also doing error handling and printing out for the user in case the wav file he inputs is either missing an important part of the FileHeader(Like the type of WAVE format) or whether there is incorrect information( Like there being data past the expected size of file).
    ##### :warning: List of error cases:
    I. In the case the characters "RIFF" are not present
    II. In the case the characters "WAVE" are not present
    III. In the case the characters "fmt " are not present
    IV. In the case the size of format chunk isn't 16
    V. the case the WAVE format isn't 1 
    VI. In the case instead of the value of 1(For mono) and 2(for stereo) the file has some other value(Like 3 for example)
    VII. In the case the bytes per second arent equal to: sample rate X block alignment
    VIII. In the case the bits per sample arent 8 or 16
    IX. In the case the block alignment does not equal to: bits per sample/8 X Mono/Stereo
    X. In the case the characters "data" are not present
    XI. In the case there is an insufficient number of samples 
    XII. In the case there is data past the expected end of file.
    #### Normal info output!
    ![Good wav](gif_folder/goodwav.gif)

    #### Possible error output!
    ![Bad_wav](gif_folder/bad_riff.gif)


    #### How does the Info subprogram accomplish this?
    Info manages to check whether the wav file is of the correct format and whether something is missing by using a number of integrated C functions and some others that I came up with.
     Integrated functions used:
     - Getchar()in order to read the actual bytes that are inputed from the wav file.
     - fprintf() in order to output both in the stdout and stderr streams, the actual info  and the errors respectively.
     - Strcmp() in order to compare strings.
    
    Functions made for the specific program:
    ``` C
        //The Reading_Words function//
        int8_t reading_Words(char x[5]){
     char Words[5];               
    for ( int i= 0; i<4; i++){
        Words[i]= getchar();
    }
    if( strcmp(x,Words) != 0){ 
        fprintf(stderr,"Error! \"%s\" not found\n",x);
        return 1;
    }
    return 0;
    }

    ```
    Which receives as an argument a string or the "Word" we want to make sure is in the file, and prints an error if it compares the existing bytes with the expected ones finds any differences.
    ``` C
        // The converting bytes function//
        int32_t converting(int8_t num){
    int8_t ch;
    int32_t size = 0;
    for (int i = 0 ; i<num; i++){
        ch = getchar();                  
        size |= (unsigned char)ch << i*8; 
    }
    return size;
    }

    ```
    This functions has as its goal the converting of the bytes it reads, from the [Little-endian format](https://en.wikipedia.org/wiki/Endianness). 
    Specifically it receives as an argument the number of bytes it will be converting. It then reads from the stream with getchar() 1 byte at a time and by using the bitwise OR (|) operator in combination with  shifting each number 8 bits to the left it manages to convert any number from little-endian to normal hexadecimal.
    **This** is done due to the fact with the bitwise OR operator ,the current numbers bits are ***"fused"*** with the fusion of all the previous ones!
    <br>


### :timer_clock: Rate
-  The **rate** subprogram, receives as input a ***wav*** file and as a second argument a float type of number and returns as output the speed up or slowed down version of the inputed wav file. In addition to that, it also checks with some slight error handling, whether the wav file is of the correct form (similar to the info subprogram).
    #### Examples
    Case 1: A file has its rate increased by 2.0(double the rate).
    ![Rate_2.0](gif_folder/rate_2.0.gif)
    Case 2: A file has its rate decreased by 0.5(half the rate).
    ![Rate_0.5](gif_folder/rate_0.5.gif)
    #### How does the Rate subprogram accomplish this?
    Rate manages to do this by altering both the Sample Rate and the Bytes per Second of the wav file and by outputing both the FileHeader (with the changed values) and the original Sample Data.
    ##### Functions used:
     Integrated C functions:
     - Getchar()
     - Putchar()
     - fprintf()
     Which are used in the same way as in the Info subprogram.
     - atoi() / atof() which are used in order to convert the string the user inputs as a second argument, into a either an integer for atoi() or a double for atof()



    Functions made for the subprogram and other down the line:
    ```C 
    //Function to input words in the wav file//
        void inputing_words(char Words[4]){   
            for (int8_t i =0; i<4;i++){
                putchar(Words[i]);
            }
        }
    ```

    Which has the purpose of inputing into the wav file, words like: "RIFF" , "data" , "WAVE" etc. Which word is inputed is decided by the argument the function receives when it is used.

    ```C
        //Function to input the bytes back into little indian//
        void inputing_endianess(int32_t To_be_converted,int8_t num){  
            int32_t bytes;
            for (int8_t i =0; i<num;i++){
                bytes = (To_be_converted>>(i*8)) & 0xFF;
                putchar(bytes);
            }

        }
    ```

    This functions, has a very important task. To take as arguments both a variable of either 32 or 16 bits(to_be_converted) and an argument for the amount of bytes it works with(num) and convert them into little endian and return them as output for the new wav file.
    This is done by shifting the first argument i*8 bits right and using the the bitwise operator AND(&) to keep the first 8 bits (1 byte) and lastly using putchar() to output the value.This process repeats a (num) amount of times and by doing this i manage to output values like the : Size of File, the Size of Data, the Mono/Stereo value etc.
    <br>

    Lastly ***rate*** modifies the SampleRate and the Bytes per Second by multipling them with the second argument the user inputs after the *rate* argument.
    <br>


### :train: Channel
- The  ***channel*** subprogram like the previous 2 receives as input a .wav file and as a secondary argument the words "left" or "right" in order to determine which channel the sound is going to go through in the case the .wav file using Stereo Sound. In the case where the soundfile is Mono instead of Stereo, the output is the same as the input (the samples are channeled through the single channel). Like the rate subprogram, channel also has some minor error handling to make sure the file that is inputed is of the correct form.

    #### Example:
    ![Channel left](gif_folder/channel_right.gif)

    #### How does the Channel subprogram accomplish this?
    Channel manages to do this by modifing a number of bytes and samples of the original .wav file. 
    Specifically ,regarding the bytes ,when the .wav file is of the Stereo type, the "Bytes per Second" , "block alingment" and the "Size of Data" are cut in half(divided by 2) while the Size of File is reduced by the now halved Size of Data.Lastly the Mono/Stereo format is converted from Stereo to Mono in this case.
    On the other hand,regarding the SampleData, the subprogram, chooses, when the bits per sample equal 8, Using a loop , a single sample (1 byte) from the left or a single sample (1byte) from the right and returns it as output for the new .wav file, until it reaches the EOF part of the original input.When the bits per sample equal 16, the subprogram chooses 2 bytes (1 sample) either from the left, or from the right as a pair, and outputs them into the new .wav file similary to the 8bits per sample case.
    ##### Functions used:
    Integrated C functions:
    - Getchar()
    - Putchar()
    - fprintf()
    Which are used in the same way as in the previous  subprograms.

    Functions made for previous subprograms:
     - inputing_endianess()
     - inputing_words()
     Which execute the same tasks as in the rate subprogram.
    <br>

### :loud_sound: Volume
-  The ***volume*** subprogram, receives like the previous ones a .wav file as input and as a secondary argument the double type number which corresponds to the increase or decrease in the volume of the file.Like all the subprograms besides it, it has some minor error handling for safety.
Unlike the previous 3 subprograms, Volume doesnt change the FileHeader of the original file, and only changes the SampleData!
    
    #### Example:
    ![Volume!](gif_folder/volume.gif)

    #### How does the Volume subprogram accomplish this?
    Volume manages this by multiplying the existing samples with the number given as a second argument.The key difference is whether the file has 8bits per sample or 16bits per sample.For the latter the volume has an increased range that the former doesnt have and that makes the execution for each different.

    ##### Functions used:
    Integrated C functions:
    - Getchar()
    - Putchar()
    - fprintf()
    - atof()
    - atoi()
    Which are used in the same way as in the previous  subprograms.

    Functions made for previous subprograms:
     - inputing_endianess()
     - inputing_words()
     Which execute the same tasks as in the rate subprogram.
     <br>

### :bulb:Generate
The ***generate*** subprogram contrary to all the previous subprograms,doesnt receive as input any wav files, but instead produces it's own output(wav file) according to this mathematical equation : 
f(t) = trunc(amp · sin(2·π·fc·t − mi·sin(2·π·fm·t))).
The user besides the argument of generate, can input 6 different values by writing the following extra arguments:

--dur int_duration : Which determines the duration of the sound in seconds(An integer).Default: 3.

--sr int_sr: Which determines the Sample rate of the file(An integer).Default: 44100.

--fm fp_fm:Which determines the frequency modulation of the signal (a double).Default: 2.0.

--fc fp_fc Which determines the carrier frequency of the signal (a double).Default: 1500.0.

--mi fp_mi: Which determines the modulation index of the signal (a double).Default: 100.0.

--amp fp_amp:Which determines the amplitude of the signal (a double).Default: 30000.0.

The sound is produced in Mono(a single channel) and using 16bits per sample.

The variable "t":This variable is the product of the (i* duration)/Sample Rate where 'i' is the current sample number (2nd, 250th, 10002349th).This way the function f(t) which produces samples, can produce a sample for the entire duration according to the sample rate(When we reach the final sample, "t" will equal the duration because t = data_chunk/2 = duration * sample rate).
### Examples
 ![Generate!!](gif_folder/generate.gif)

#### How does the Generate subprogram accomplish this?
Channel manages to do this by combining function from all the other subprograms.It inputs the FileHeader, and then in a loop,it starts adding Samples until the loop ends!
Regarding the actual FileHeader and how each part is calculated (The Size of File, the Data_Chunk etc.)
They are calculated like this:
```c
    int32_t data_chuck= sr*dur*2;
    int32_t size_of_file = data_chuck+36;
    int16_t bits_per_sample = 16;
    int16_t block_align=bits_per_sample/8 * 1;
    int32_t bytes_sec = sr * block_align;
```
  - sr = Sample Rate
  - dur = Duration

    ##### Functions used:
    Integrated C functions:
    - Getchar()
    - Putchar()
    - fprintf()
    - atoi()
    Which are used in the same way as in the previous  subprograms.
    -  strtod() which serves a similar purpose to atof, with the added benefit that the error handling is easier and has a wider range .

    Functions made for previous subprograms:
     - inputing_endianess()
     - inputing_words()
     Which execute the same tasks as in the rate subprogram.
    <br>
-----------------------------------------------------------
## :mag: 3 . Libraries used, Compiler and How to run!

The libraries used in the making of this program are the following: 
``` C
        #include <stdio.h>
        #include <string.h>
        #include <math.h>
        #include <stdlib.h>
        #include <stdint.h>
        #include <errno.h>
```
While also defining :
```c
    #define M_PI
```
The compiler used is the [GC compiler](https://en.wikipedia.org/wiki/GNU_Compiler_Collection) and the command to compile the program is the following:
 ```
$gcc -Ofast -Wall -Wextra -Werror -pedantic -o soundwave soundwave.c -lm
```

## Sources
-------------------------------------------------
- [Wave file format document.](https://docs.fileformat.com/audio/wav/)
- [Wikipedia Endianess format](https://en.wikipedia.org/wiki/Endianness)
- More Stack Overflow posts than i can count

