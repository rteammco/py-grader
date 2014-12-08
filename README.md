py-grader
=========

This is simple script for easily interacting with basic Python 3 programs and grade them. It can be useful for grading or self-checking assignments in introductory courses that use Python 3. The program will generate a .csv spreadsheet with all of the grades for each student.



Overview
--------------

Everything that "grader" needs to run is defined the the module file (an example is provided in the file "a9mod"). This file specifies the grading directory, the name of the Python file to run, the due date, and how many times the file should be executed (more than once if you want to test the program with different input sets).

The grading directory should only contain subdirectories, each subdirectory associated with a different student. All the files that are to be graded need to be in each student's subdirectory.

For example:

<pre>
$ cd /path/to/gradingdir
$ mkdir hw1
$ cp -r ~/Downloads/Assignments/"Doe, John" ./hw1
$ cp -r ~/Downloads/Assignments/"Who, Jane" ./hw1
</pre>

Now create your hw1mod file (see below) and run `$ ./grader hw1mod`.



The "mod" File
--------------

The top of the "mod" file contains the following information (separated by lines):

<ol>
  <li>
    The name of the grading directory (e.g. <code>hw1</code> in the example above).
  </li>
  <li>
    The Python file that will be executed (e.g. <code>Homework1.py</code>).
  </li>
  <li>
    The due date: two integers, the day followed by the month (e.g. <code>14 9</code> for September 9th).
  </li>
  <li>
    An integer specifying how many times the program will be tested. For each test, you can specify different inputs to the program.
  </li>
</ol>

Next, specify the inputs and outputs. For each test, the following must be specified in this order:

<pre>
INPUT
>>> console input 1
>>> console input 2
>>> etc.
INFILE infilename.txt
OUTPUT keyword1 keyword2 etc.
>>> console output line 1
>>> console output line 2
>>> etc.
OUTFILE outfilename.txt
>>> file output line 1
>>> file output line 2
>>> etc
</pre>

You do not need to include all of the parts (INPUT, INFILE, OUTPUT, OUTFILE), but those that you do specify must be in the above order. The parts with the `>>> ` symbols indicate the input that will be fed into the program, or else the desired output. "infilename.txt" is an input file. Keep this file in the same directory as the grader script. It will automatically be copied over to each student's directory as needed. The file "outfilename.txt" is the name of the output file, as expected. This means that when a test is over, the grading program expects to see a file called "outfilename.txt" in that student's directory with the output matching the specified lines. For OUTPUT, you can also provide the keywords. Keywords indicate values that must be matched 100% for a correct answer. If these values are not found in the output, the answer is wrong. The output itself will only test for formatting accuracy.



Running "grader"
--------------

<b> To run the program, type: </b> <br>
`$ ./grader modfile [options]` <br>

<b> Optional arguments: </b>
<ul>
  <li>
    <code> -s start </code> - The grader traverses the grading directory in alphabetical order, so specifying "start" is the starting directory that the grader will pick up at. For example, if you already graded "Doe, John", starting at "Who" will start grading at "Who, Jane" and move down the list.
  </li>
  <li>
    <code> -i </code> - Ignores spacing when checking program or file outputs. This is less strict when it comes to output formatting.
  </li>
  <li>
    <code> -p </code> - Sets partners enabled. Using this option will scan the file header comments for the keyword "partner" and assign the same grade to two people who partnered together. This saves time on grading.
  </li>
</ul>



Using "grader"
--------------

Once everything is set up and you have the program running, you can begin grading. The program will display the file, followed by any errors it automatically detected. Points will be subtracted automatically based on the mistakes and malformed output the grader detected.

Once grading for that student is finished, you may choose to update files. A numbered list of issues where points were taken off will appear in red, followed by a recommended max grade in green. For the grade, hit ENTER to assign the suggested grade. You can also type in a different grade to assign that instead. Use + or - followed by a number to add or subtract that many points. Use x followed by a number to remove that mistake from the list.

For example, if this is the given output:

<pre>
[1] Wrong answer(s): -20
[2] Mismatched output: -10
To remove an issue, type x# where # is the issue number.
Add or subtract points using +N or -N (N is the number of points).
If you want to grade this manually, type in the letter 'm'.
Max grade: 70
Grade:     
</pre>

and you don't think the output was mismatched, and you also want to award 5 extra points, type:

`x1 +5`

This will remove condition #1 and also add 5 points to the final grade, resulting in a new suggested grade query:

<pre>
[1] Mismatched output: -10
To remove an issue, type x# where # is the issue number.
New grade: 95
Grade:     
</pre>

Now hit ENTER to confirm this grade. You can now choose to add your own comments. Note that the mistakes from the numbered list in the last step will be automatically added as comments for why points were taken off.

Here's the full interaction:

<pre>
[1] Wrong answer(s): -20
[2] Mismatched output: -10
To remove an issue, type x# where # is the issue number.
Add or subtract points using +N or -N (N is the number of points).
If you want to grade this manually, type in the letter 'm'.
Max grade: 70
Grade:     x1 +5


[1] Mismatched output: -10
To remove an issue, type x# where # is the issue number.
New grade: 95
Grade:     
Comments:  Some bonus points added!
[['Doe, John'], '95', 'Mismatched output [-10] Some bonus points added!']
Done. Type 'r' to redo this grade, or 'q' to quit. 
</pre>

You can hit ENTER to confirm this grade and comments. Enter "q" instead to quit (the grade will still be saved). Enter "r" to redo this grade if you messed up (the grade won't be saved until you regrade it).
