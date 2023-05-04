Download Link: https://assignmentchef.com/product/solved-csci1302-minesweeper-alpha
<br>
<h2>Project Description</h2>

This first project is meant to ensure that you are able to apply and extend your prerequisite knowledge as well as introduce you to developing and testing a Java application in a Linux environment (i.e., the Odin development server). You should be familiar with the majority of aspects of this project through your introductory programming course and the work so far from 1302. However, you may also be asked to do things that you have never been given explicit directions for before. This is just a part of software development. Sometimes you need to research how to solve a problem in order to implement a solution. That being said, the material included in this document should hopefully answer the majority of your questions.

Your goal is to develop a <strong>non-recursive</strong>, <strong>non-GUI</strong> (GUI = Graphical User Interface) version of the game called <strong>Minesweeper</strong>. The code for this game will be organized in such a way that the recursive elements of Minesweeper can be added at a later point in time, if desired. It will also be organized so that you can add a GUI to it later as well. Interestingly, the organization of some of the classes in this project will also introduce you to some elementary aspects of game programming.

If you are not familiar with Minesweeper as a game, we recommend playing a free, online version of the game. Just make sure to ignore the recursive elements of the game that uncovers more than one cell in a single click (more below). Our version will only do one cell at a time. After you’ve familiarized yourself with the game, play the oracle a few times to see the expected 1302 implementation. You should also consult the <a href="https://en.wikipedia.org/wiki/Minesweeper_(video_game)" rel="nofollow">Wikipedia entry</a> for the game, ignoring any mentions of recursion. Once you’re familiar with the basic gameplay, then continue reading this project description.

<h3><a id="user-content-note-concerning-no-recursion" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#note-concerning-no-recursion" aria-hidden="true"></a>Note Concerning “No Recursion”</h3>

In a traditional game of Minesweeper, when the player “reveals” a square that does not contain a mine, <strong>two</strong> things happen:

<ol>

 <li>A number representing the number of mines in the (up to) eight adjacent squares is placed in the revealed square; and</li>

 <li>If the number of adjacent mines is zero, then game goes ahead and “reveals” all of the (up to) eight adjacent squares.</li>

</ol>

The second part mentioned above can cause one reveal made by the user to result in multiple reveals in the minefield. This behavior is what the literature is referring to when it talks about recursion in Mineweeper. <strong>Your game should not support this behavior</strong>. If the user reveals one square, then, at most, one square is revealed in the minefield.

<h3><a id="user-content-minesweeper-overview" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#minesweeper-overview" aria-hidden="true"></a>Minesweeper Overview</h3>

In your Minesweeper, the player is initially presented with a grid of undifferentiated squares. Some of those squares contain hidden mines. The size of the grid, the number of mines, and the individual mine locations are set in advance by a seed file (more on that later) that the user specifies as a command-line argument to your program. The ratio of the number of mines to the grid size is often used as a measure of an individual game’s difficulty. The grid size can also be represented in terms of the number of rows and columns in the grid. In this project description, we may refer to the <em>grid</em> or to the <em>minefield</em>. Both of these terms mean the same thing. Furthermore, we will use the term <em>square</em> to denote a location in the minefield, even in situations where a location may be visually rectangular instead of perfectly square.

The game is played in rounds. During each round, the player is presented with the grid, the number of rounds completed so far, as well as a prompt. The player has the option to do 6 different things, each of which is briefly listed below and explained in great detail in later sections:

<ol>

 <li>Reveal a square on the grid.</li>

 <li>Mark a square as potentially containing a mine.</li>

 <li>Mark a square as definitely containing a mine.</li>

 <li>Lift the fog of war (cheat code).</li>

 <li>Display help information.</li>

 <li>Quit the game.</li>

</ol>

When the player reveals a square of the grid, different things can happen:

<ul>

 <li>If the revealed square contains a mine, then the player loses the game.</li>

 <li>If the revealed square does not contain a mine, then a digit is instead displayed in the square, indicating how many adjacent squares contain mines. Typically, there are 8 squares adjacent to any given square, unless the square lies on an edge or corner of the grid. The player uses this information to deduce the contents of other squares, and may perform any of the first three options in the list presented above.</li>

 <li>When the player marks a square as potentially containing a mine, a <code>?</code> is displayed in the square. This provides the user with a way to note those places that they believe may contain a mine but are not sure enough to mark as definitely containing a mine.</li>

 <li>When the player marks a square as definitely containing a mine, a flag, denoted by the character <code>F</code>, is displayed in the square.</li>

</ul>

To simplify the game mechanics, <strong>the player may mark, guess, or reveal any square in the grid, even squares that have already been marked or revealed.</strong> In other words, the player may issue a command to mark, guess, or reveal a square, regardless of its current state. The logic for determining what happens to the square is always the same. For example, if a square has been revealed and the user marks it as definitely containing a mine then a round is consumed and the square should be marked. The user would then have to reveal this square again later. This may not be consistent with how you’ve played Minesweeper in the past but it will make it easier to code. We will leave it up to the user to be smart about how they play!

The game is won only when <strong>both</strong> of the following conditions are met:

<ul>

 <li>All squares containing a mine are marked as <em>definitely</em> containing a mine; and</li>

 <li>All squares not containing a mine are revealed.</li>

</ul>

At the end of the game, the player is presented with a score. Let <code>rows</code>, <code>cols</code>, and <code>rounds</code> denote the number of rows in the grid, columns in the grid, and number of rounds completed, respectively. A <strong>round</strong> is defined as one successful iteration of the main game loop. Therefore, only valid commands result in a round being consumed. To be clear, <em>rounds</em> is not quite the same as the number of commands entered (some may be invalid); however, it should be less than or equal to that number.

The player’s score is calculated as follows:

A score of <code>100</code> would denote a perfect game. In this version of Mineweeper, it should not be possible for the player to win the game in less than <code>(rows * cols)</code>-many rounds (take a second to convince yourself of this fact). Therefore, any game in which the player exceeds that many rounds would result in a score that is less than <code>100</code>. When displaying the score, the number should always be printed with two digits following the decimal point.

<h3><a id="user-content-win-conditions"></a><a id="user-content-the-grid-and-interface" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#the-grid-and-interface" aria-hidden="true"></a>The Grid and Interface</h3>

When the game begins, the following <strong>welcome banner</strong> should be displayed to the player once and only once:

<a id="user-content-win-conditions"></a>Take care when printing this message out to the screen. Depending on how you implement this part, you may need to escape some of the characters in order for them to show up correctly. A copy of this welcome banner is contained in this <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha/blob/master/README.md"><code>README.md</code></a> file and in <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha/blob/master/resources/welcome.txt"><code>resources/welcome.txt</code></a>.

In this Minesweeper game, the initial game configuration is loaded from a <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#seed-files">seed file</a>; the player provides the path to a <em>seed file</em> when as a command-line argument to the program. Two pieces of of information that are read from the seed file are the number of rows and the number of columns which together specify the grid size (i.e., the size of the minefield).

The number of rows and the number of columns need not be the same. Rows and columns are indexed starting at <code>0</code>. Therefore, in a <code>10</code>-by-<code>10</code> (rows-by-columns), the first row is indexed as <code>0</code> and the last row is indexed as <code>9</code> (similarly for columns). In a <code>5</code>-by-<code>8</code> game, the row indices are from <code>0</code> to <code>4</code>, while the column indices are from <code>0</code> to <code>7</code>, respectively.

<h4><a id="user-content-the-grid" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#the-grid" aria-hidden="true"></a>The Grid</h4>

Let’s assume we are playing a <code>5</code>-by-<code>5</code> game of Minesweeper. When the game starts, the interface should look like this:

Let’s assume we are playing a <code>10</code>-by-<code>10</code> game of Minesweeper. When the game starts, the interface should look like this:

Please note that the in either example, the first, third, and second-to-last lines are blank (the lines before and after “Rounds Completed” and the line before the prompt). All other lines, except the last line containing the prompt, start with one blank space. The line containing the prompt contains an extra space after the <code>:</code> so that <strong>when the user types in a command, the text does not touch the <code>:</code>.</strong> Multiple output examples are provided in the <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#minefield-output-examples">Appendix</a> of this project description for your convenience.

<h4><a id="user-content-the-user-interface" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#the-user-interface" aria-hidden="true"></a>The User Interface</h4>

The possible commands that can be entered into the game’s prompt as well as their syntax are listed in the subsections below. Commands with leading or trailing whitespace are to be interpreted as if there were no leading or trailing whitespace. For example, the following two examples should be interpreted the same:

Although it’s hard to see in the example above, trailing whitespace should also be ignored. That is, if the user types <code></code>one or more times before pressing the <code>RET</code> (return) key, then those extra whitespaces should be ignored.

The different parts of a command are known as tokens. The <code>help</code> command, for example, only has one token. Other commands, such as the <code>mark</code> (seen below) have more than one token because other pieces of information are needed in order to interpret the command. As a quick example (which will be explored in more depth below), the player can mark the square at coordinate (0,0) using <code>mark</code> as follows:

In the above example, you can see that the <code>mark</code> command has three tokens. A command with more than one token is still considered syntactically correct if there is more than one white space between tokens. For example, the following four examples should be interpreted the same:

As a reminder, trailing whitespace is ignored.

<h4><a id="user-content-advice-on-reading-commands" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#advice-on-reading-commands" aria-hidden="true"></a>Advice on Reading Commands</h4>

All valid game commands are entered on a single line. Implementers should always use the <code>nextLine()</code> method of their one and only <em>standard input</em> <code>Scanner</code> object to retrieve an entire line of input for a command as a <code>String</code>. Once an entire line is retrieved, it can be parsed using various methods; however, implementers may find it useful to construct a new <code>Scanner</code> object using the line as its source so that they can scan over the individual tokens. To put this into perspective, taking the “make a <code>Scanner</code> from the line” approach would make it so you can handle all four examples at the end of the last sub-section with one set of code.

Here’s some example code:

<h4><a id="user-content-command-syntax-format" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#command-syntax-format" aria-hidden="true"></a>Command Syntax Format</h4>

In the sections below, we describe the syntax format that each command must adhere to in order to be considered correct. Unknown commands and commands that are known but syntactically incorrect are considered invalid. Information about displaying errors related to invalid commands is contained in <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#displaying-errors">a later section</a> in this document.

<strong>Please do not confuse this syntax with regular expressions, a topic that will not be covered in this course.</strong> You are NOT meant to put this weird looking syntax into any code. It is purely meant to convey to you, the reader, what is and what is not valid input for a given command.

In a syntax format string, one or more non-new-line white space is represented as a <code>-</code>. Command tokens are enclosed in <code>[]</code> braces. If the contents of a token are surrounded by <code>""</code> marks, then that token can only take on that literal value. If more than one literal value is accepted for a token, then the quoted literals are separated by <code>/</code>. If the contents of a token are surrounded by <code>()</code> marks, then that token can only take on a value of the type expressed in parentheses. Note: the literal values are case-sensitive. So, “ReVeal” is not the same as “reveal”.

<h4><a id="user-content-revealing-a-square" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#revealing-a-square" aria-hidden="true"></a>Revealing a Square</h4>

In order to reveal a square, the <code>reveal</code> or <code>r</code> command is used. The syntax format for this command is as follows: <code>-["reveal"/"r"]-[(int)]-[(int)]-</code>. The second and third tokens indicate the row and column indices, respectively, of the square to be revealed.

Let’s go back to our <code>10</code>-by-<code>10</code> example. Suppose that we secretly know that there is a mine in squares (1,1) and (1,3). Now suppose that the player wants to reveal square (1, 2). Here is an example of what that might look like.

After the player correctly entered the command <code>r 1 2</code>, the state of the game updates (e.g., number of rounds completed, the grid, etc.), and the next round happens. Since there was no mine in square (1,2), the player does not lose the game. Also, since the total number of mines in the 8 cells directly adjacent to square (1,2) is 2, the number 2 is now placed in that cell.

If the player reveals a square containing a mine, then the following message should be displayed and the program should exit <em>gracefully</em> (as defined near the end of this section):

Yeah, that’s old school ASCII art. Please note that the first and last lines are blank. Also note that the second line (containing “oh no…”) begins with a single white space. A copy of this game over text, excluding the first and last blank lines, is contained in <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha/blob/master/resources/gameover.txt"><code>resources/gameover.txt</code></a>.

<a id="user-content-graceful-exit"></a><strong>Graceful Exit:</strong> When we say that a program should exit <em>gracefully</em>, we mean that the <em>exit status</em> code used in the call to <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html#exit(int)" rel="nofollow"><code>System.exit</code></a> is <code>0</code> (i.e., zero).

<ul>

 <li>If a <em>graceful</em> exit is expected and your program exits for any reason with an exit status other than <code>0</code> (e.g., if your game crashes), then some points will be deducted from your grade.</li>

 <li>Immediately after any program terminates and returns to the terminal shell, a user can inspect what exit code was used by executing the following command:Note that using <code>echo $?</code> a second time would show the exit status of the first <code>echo</code> command; you would need to rerun your program and cause it to exit in order to check the exit status again.</li>

</ul>

<h4><a id="user-content-mark-command" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#mark-command" aria-hidden="true"></a>Mark Command</h4>

In order to mark a square as definitely containing a mine, the <code>mark</code> or <code>m</code> command is used. The syntax format for this command is as follows: <code>-["mark"/"m"]-[(int)]-[(int)]-</code>. The second and third tokens indicate the row and column indices, respectively, of the square to be revealed.

Let’s go back to our <code>10</code>-by-<code>10</code> example. Suppose that the player wants to mark square (1, 2). Here is an example of what that might look like.

After the player correctly entered the command <code>m 1 2</code>, the state of the game updates (e.g., number of rounds completed, the grid, etc.), and the next round happens.

<h4><a id="user-content-guess-command" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#guess-command" aria-hidden="true"></a>Guess Command</h4>

In order to mark a square as potentially containing a mine, the <code>guess</code> or <code>g</code> command is used. The syntax format for this command is as follows: <code>-["guess"/"g"]-[(int)]-[(int)]-</code>. The second and third tokens indicate the row and column indices, respectively, of the square to be revealed.

Let’s go back to our <code>10</code>-by-<code>10</code> example. Suppose that the player wants to guess square (1, 2). Here is an example of what that might look like.

After the player correctly entered the command <code>g 1 2</code>, the state of the game updates (e.g., number of rounds completed, the grid, etc.), and the next round happens.

<h4><a id="user-content-no-fog-command" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#no-fog-command" aria-hidden="true"></a>No Fog Command</h4>

This command removes, for the next round only, what is often referred to as the, “fog of war.” All squares containing mines, whether unrevealed, marked, or guessed, will be displayed with less-than and greater-than symbols on either side of the square’s center (as opposed to white space). Using the <code>nofog</code> command <strong>does</strong> use up a round. In order to issue this command, the <code>nofog</code> command is used. The syntax format for this command is as follows: <code>-["nofog"]-</code>.

Let’s go back to our <code>10</code>-by-<code>10</code> example. Suppose that in this example, there are only two mines in the entire board which are located in squares (1,1) and (1,3). If the player marked square (1,1) during the first round and then used the <code>nofog</code> command during the second round, then here is an example of what that scenario might look like:

<strong>NOTE:</strong> This command should <strong>not</strong> be listed when the <code>help</code> command is used. Think of it as a cheat code! It should also be useful for debugging.

<h4><a id="user-content-help-command" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#help-command" aria-hidden="true"></a>Help Command</h4>

In order to show the help menu, the <code>help</code> or <code>h</code> command is used. The syntax format for this command is as follows: <code>-["help"/"h"]-</code>.

Let’s go back to our <code>10</code>-by-<code>10</code> example. Suppose that the player wants to display the help menu. Here is an example of what that might look like.

After the player correctly entered the command <code>h</code>, the state of the game updates (e.g., number of rounds completed, the grid, etc.), the help menu is displayed, and the next round happens.

<strong>Note:</strong> the <code>help</code> command does use up a round.

<h4><a id="user-content-quit-command" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#quit-command" aria-hidden="true"></a>Quit Command</h4>

In order to quit the game, the <code>quit</code> or <code>q</code> command is used. The syntax format for this command is as follows: <code>-["quit"/"q"]-</code>.

Let’s go back to our <code>10</code>-by-<code>10</code> example. Suppose that the player wants to quit the game. Here is an example of what that might look like.

After the player correctly entered the command <code>q</code>, the game displayed the goodbye message and the program exited <em>gracefully</em> (as defined elsewhere in this document).

<h4><a id="user-content-player-wins" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#player-wins" aria-hidden="true"></a>Player Wins</h4>

When the player wins the game, the following message should be displayed to the player and the game should exit <em>gracefully</em> (as defined elsewhere in this document):

Note that the first and last lines are blank and that the beginning of the other lines contain a single white space. You should replace the score in the output with the actual calculated score (mentioned above). A copy of this game won text, excluding the first and last blank lines as well as the score value, is contained in <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha/blob/master/resources/gamewon.txt"><code>resources/gamewon.txt</code></a>.

The conditions for winning are outlined earlier in this document, <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#win-conditions">here</a>.

<h3><a id="user-content-seed-files" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#seed-files" aria-hidden="true"></a>Seed Files</h3>

Each game is setup using seed files. Seed files have the following format:

<ul>

 <li>The first two tokens are two integers (separated by white-space) indicating the number of <code>rows</code> and <code>cols</code>, respectively, for the size of the mine board.</li>

 <li>The third token is an integer indicating <code>numMines</code>, i.e., the number of mines to be placed on the mine board.</li>

 <li>Subsequent pairs of tokens are integers (separated by white space) indicating the location of each mine.</li>

</ul>

<strong>NOTE:</strong> Please refer to the “<strong>Seed File Malformed Error</strong>” description in the <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#displaying-errors">“Displaying Errors” section</a> later in this document for constraints related to the tokens in a seed file.

<strong>NOTE:</strong> In Java, the term <em>white-space</em> refers to one or more characters in a sequence that each satisfy the conditions outlined in <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/Character.html#isWhitespace(char)" rel="nofollow"><code>Character.isWhitespace</code></a>. You do not need to check these conditions specifically nor use this method if you use the built-in tokenizing provided by the <code>Scanner</code> class.

<strong>NOTE:</strong> It is acceptable for white-space to occur both at the beginning and end of a seed file.

The following seed files are valid and contain the same information:

An example seed file is present in the project materials. In order to run your program with the seed file, you should be able to use the following command (actual seed filename may differ):

<strong>Note:</strong> The command you use to run your file from your main project directory (the directory containing <code>src</code> and <code>bin</code>) should <strong>exactly match</strong> the command above if you are passing in a seed file called <code>seed1.txt</code> containined in a <code>tests</code> directory located directly within your main project directory.

To read the file, let us assume that we have access to the seed file’s path via a <code>String</code> variable called <code>seedPath</code> and that we will use the following classes:

<ul>

 <li><a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/File.html" rel="nofollow"><code>File</code></a></li>

 <li><a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Scanner.html" rel="nofollow"><code>Scanner</code></a></li>

</ul>

Most of you have used the <code>Scanner</code> class to read keyboard input from standard input. Here, we will use it to read from a text file. This is accomplished using something similar to the following code snippet:

You may need to import <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/FileNotFoundException.html" rel="nofollow"><code>FileNotFoundException</code></a> (or use its fully qualified name) if adapting the code snippet above.

<h3><a id="user-content-displaying-errors" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#displaying-errors" aria-hidden="true"></a>Displaying Errors</h3>

All error messages should be printed to standard error (<code>System.err</code>) with one blank line preceeding the line with the error message. In some cases, an error message should cause the program to terminate. In such cases, an integer will be specificied that you are required to use the specific exit status number in your call to <a href="https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html#exit(int)" rel="nofollow"><code>System.exit</code></a>. If you let <code>errorMessage</code> denote an error message and <code>exitStatus</code> denote an exit status, then here is some code that exactly illustrates what needs to happen for error messages that exit the program:

If an error does not exit the program, then the last line in the code above should be omitted.

Here is a list of the different errors that can occur in the program:

<ul>

 <li style="list-style-type: none;">

  <ul>

   <li><strong>Invalid Usage Error:</strong> If your program encounters any number of command-line arguments other than one (i.e., if <code>args.length != 1</code>), then the error message and exit status in the table near the end of this section should be used.</li>

   <li><strong>Seed File Not Found Error:</strong> If your program is not able to read the seed file that is specified by the user via a command-line argument due to a <code>FileNotFoundException</code>, then the error message and exit status in the table near the end of this section should be used.Note that <code>&lt;message&gt;</code> (including the angle brackets) in the error message text should be replaced with the <code>String</code> returned by the exception object’s <code>getMessage()</code> method.</li>

   <li><strong>Seed File Malformed Error:</strong> <a id="user-content-badseed"></a>If your program encounters a problem in the format of a seed file, then the error message and exit status in the table near the end of this section should be used. Note that a seed file is considered <em>malformed</em> or not formatted correctly if any of the following conditions are met:

    <ul>

     <li>a token is expected but is not found;</li>

     <li>a token is not of the expected type (e.g., it’s expected to be an <code>int</code> but it’s not);</li>

     <li>the token for <code>rows</code> is less than <code>5</code> or greater than <code>10</code>;</li>

     <li>the token for <code>cols</code> is less than <code>5</code> or greater than <code>10</code>;</li>

     <li>the token for <code>numMines</code> is less than <code>1</code> or greater than <code>(rows * cols) - 1</code>; or</li>

     <li>the location of a mine is not in bounds.</li>

    </ul>Note that <code>&lt;message&gt;</code> (including the angle brackets) in the error message text should be replaced with some descriptive <code>String</code>. If the error arises due to some exception, then you may use the <code>String</code> returned by the exception object’s <code>getMessage()</code> method; otherwise, you may use some short, single line <code>String</code> of your choosing that describes the problem.</li>

  </ul></li>

</ul>

<ul>

 <li><strong>Invalid Command Error:</strong> While the game is running, if a command entered by the player is invalid or not recognized, then the error message in the table near the end of this section should be used. For this kind of error, the program <strong>should NOT exit</strong>, and <strong>a round should NOT be consumed</strong> (i.e., your counter for the number of rounds should not increase). After the error message is displayed, the round is essentially restarted and the number of rounds, the grid, and the prompt should be displayed again.Note that <code>&lt;message&gt;</code> (including the angle brackets) in the error message text should be replaced with some descriptive <code>String</code>. If the error arrises due to some exception, then you may use the <code>String</code> returned by the exception object’s <code>getMessage()</code> method; otherwise, you may use some short, single line <code>String</code> of your choosing that describes the problem.</li>

</ul>

Here is the table that summarizes the different error messages and exit status codes:

<table>

 <thead>

  <tr>

   <th>Error</th>

   <th>Error Message</th>

   <th>Exit Status</th>

  </tr>

 </thead>

 <tbody>

  <tr>

   <td><strong>Invalid Usage Error</strong></td>

   <td><code>Usage: MinesweeperDriver SEED_FILE_PATH</code></td>

   <td><code>1</code></td>

  </tr>

  <tr>

   <td><strong>Seed File Not Found Error</strong></td>

   <td><code>Seed File Not Found Error: &lt;message&gt;</code></td>

   <td><code>2</code></td>

  </tr>

  <tr>

   <td><strong>Seed File Malformed Error</strong></td>

   <td><code>Seed File Malformed Error: &lt;message&gt;</code></td>

   <td><code>3</code></td>

  </tr>

  <tr>

   <td><strong>Invalid Command Error:</strong></td>

   <td><code>Invalid Command: &lt;message&gt;</code></td>

   <td>NA</td>

  </tr>

 </tbody>

</table>

<h2><a id="user-content-badseed"></a><a id="user-content-minesweeper-oracle" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#minesweeper-oracle" aria-hidden="true"></a>Minesweeper Oracle</h2>

If at any time while you are writing your Minesweeper program you find yourself wondering “How should my program respond if _____ happens”, you can consult the provided Minesweeper oracle. The oracle is an executable-only (no code provided) version of the instructors’ solution to the Minesweeper project and is available on Odin. To run the oracle, you need to provide the same command-line arguments that you would to your version of Minesweeper. You only need to change the start of the command. Here is the exact syntax:

where <code>some/path/to/seed.txt</code> and <code>optional/path/to/input.txt</code> are replaced with paths to real seed and input files, respectively.

If you find a bug in the oracle or believe there to be an inconsistency between what the oracle says and what this document says, please let your instructors know in a Piazza post. We have only given students access to the oracle once before so we may still have to iron out a few kinks.

<h2><a id="user-content-project-requirements--grading" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#project-requirements--grading" aria-hidden="true"></a>Project Requirements &amp; Grading</h2>

This assignment is worth 100 points. The lowest possible grade is 0, and the highest possible grade without extra credit is 100.

<h3><a id="user-content-functional-requirements" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#functional-requirements" aria-hidden="true"></a>Functional Requirements</h3>

A functional requirement is <em>added</em> to your point total if satisfied. There will be no partial credit for any of the requirements that simply require the presence of a method related a particular functionality. The actual functionality is tested using test cases.

<h4><a id="user-content-40-points-required-classes" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#40-points-required-classes" aria-hidden="true"></a>(40 points) Required Classes</h4>

<ul>

 <li><a id="user-content-badseed"></a><a id="user-content-badseed"></a><strong><code>cs1302.game.MinesweeperGame</code> Class</strong>: <a id="user-content-constructor"></a>Instances of this class represent a game of Minesweeper Alpha. You need to implement all of the methods listed below. Unless stated otherwise, each method is assumed to have public visibility. Instance variables may be added, as needed, to keep track of object state.

  <ul>

   <li><strong><code>MinesweeperGame(Scanner stdIn, String seedPath)</code></strong>: In this constructor, you should intialize some of your instance variables and setup the game using the information in the seed file referred to by <code>seedPath</code>.</li>

   <li>Keep in mind that this constructor will be called from your <code>Driver</code> class, so that’s where the iniitial values of <code>stdIn</code> and <code>seedPath</code> come from. You should refer to the instructions for <code>Driver</code> for more information before you make any assumptions.</li>

   <li>You will probably want to create a separate method for reading the seed file.</li>

   <li><a id="user-content-constructor"></a>

    <ul>

     <li>Take care that you <strong>assign <code>stdIn</code> to an instance variable</strong> and that you use that instance variable whenever you need to read from standard input.<a id="user-content-constructor"></a><strong>WARNING:</strong> Other than the initial assignment to your <code>stdIn</code>-related instance variable in the constructor, you should not reassign that instance variable elsewhere in your <code>MinesweeperGame</code> class; if you do, then you risk getting a zero on your project per <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#stdin">one of the requirements</a>.<strong>SUGGESTION:</strong> We said that you should assign <code>stdIn</code> to an instance variable, but <strong>it’s also perfectly okay for you to assign it to an instance constant</strong>. This will prevent you from reassigning it on accident. In Java, you can leave an instance constant uninitialized on the line where it’s declared so long as you initialize it exactly once inside your constructor. Here is the declaration (notice we didn’t initialize it here):Here is what a line in the constructor might look like:If you follow this suggestion, then you can call <code>stdIn.nextLine()</code> or <code>this.stdIn.nextLine()</code> in your class’s instance methods whenever you want to get a line of user input (e.g., for a command).</li>

    </ul></li>

   <li><strong><code>void printWelcome()</code>:</strong> This method should print the welcome banner to standard output, as described earlier in this document.</li>

   <li><strong><code>void printMineField()</code>:</strong> This method should print the current contents of the mine field to standard output, as described earlier in this document.</li>

   <li><strong><code>void promptUser()</code>:</strong> This method should print the game prompt to standard output and interpret user input from standard input, as described earlier in this document. Based on the command received, this method should delegate (i.e., call other methods) to handle the work.</li>

   <li><strong><code>boolean isWon()</code>:</strong> This method should return <code>true</code> if, and only if, all the conditions are met to win the game as defined earlier in this document.</li>

   <li><strong><code>void printWin()</code> and <code>void printLoss()</code>:</strong> These methods should print the win and game over emssages to standard output, respectively, as described earler in this document.</li>

   <li><strong><code>void play()</code>:</strong> This method should provide the main game loop by invoking other instance methods, as needed.</li>

  </ul><strong>NOTE:</strong> Please see the <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#suggestions">Suggestions</a> section of this document before writing the code to implement these methods.<strong>NOTE:</strong> You are encouraged to implement other methods, as needed, to help with readability, code reuse, etc. In some cases, you may need to add other methods to meet the style requirement for method length.</li>

 <li><strong><code>cs1302.game.MinesweeperDriver</code> Class</strong>: This class should only contain the <code>main</code> method:

  <ul>

   <li><code>void main(String[] args)</code>: This public, static method should do the following :

    <ol>

     <li><strong>Create your one and only standard input <code>Scanner</code> object.</strong> While your program may have as many <code>Scanner</code> objects as is needed, it may only have exactly one <code>Scanner</code> object for standard input per program execution. Your <code>main</code> method should include the following code:You must not include <code>new Scanner(System.in)</code> anywhere else in your project or you risk getting a grade of zero for the project.</li>

     <li><strong>Handle command-line arguments.</strong> Exactly one command-line argument is expected, and it represents the path to a seed file. You should assign that argument to a local <code>String</code> variable called <code>seedPath</code> so that it can be used in the call to your <code>MinesweeperGame</code> constructor in step 3.</li>

     <li><strong>Instatiate the <code>MinesweeperGame</code> object call its <code>play()</code> method.</strong> Use the <code>stdIn</code> and <code>seedPath</code> variables from the first two steps when you create the object. Once the object is created, call its <code>play()</code> method.</li>

    </ol></li>

  </ul></li>

 <li><strong>(60 points) Test Cases</strong>: <a id="user-content-test-cases"></a>The bulk of this project will be graded based on multiple test cases. A single test case can be described by three things:<a id="user-content-test-cases"></a><a id="user-content-test-cases"></a>Some example test cases are provided with the project description; they are described in the <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#test-case-examples">Test Case Examples</a> section of the Appendix. Since the actual test cases that will be used to grade your project may be a superset of the ones provided, you should take care to read and understand how to test your program using the example test cases. If your program does not execute with a command-line argument and input/output redirection as described in the appendix (and perhaps elsewhere), then it will automatically be assigned a grade of zero, regardless of any code contained in the submission. No exceptions will be made for this.To be absolutely clear, you must make sure your program runs as described in the <a href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#test-case-examples">Test Case Examples</a> section of the Appendix; <strong>the graders will not adjust the commands when running your program to accomodate a different set of command-line arguments.</strong> If there is a package-related issue, however, then the graders may make some minor adjustments for a relatively small grade penalty.</li>

</ul>

<h3><a id="user-content-non-functional-requirements" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#non-functional-requirements" aria-hidden="true"></a>Non-Functional Requirements</h3>

A non-functional requirement is <em>subtracted</em> from your point total if not satisfied. In order to emphasize the importance of these requirements, non-compliance results in the full point amount being subtracted from your point total. That is, they are all or nothing (no partial credit).

<ul>

 <li style="list-style-type: none;">

  <ul>

   <li><strong>(10 or 100 points) Project Directory Structure:</strong> The location of the default package for the source code should be a direct subdirectory of <code>cs1302-minesweeper-alpha</code> called <code>src</code>. When the project is compiled, the <code>-d</code> option should be used with <code>javac</code> to make the default package for compiled code a direct subdirectory of <code>cs1302-minesweeper-alpha</code> called <code>bin</code>.If you follow this structure, then you would type the following to compile your code, assuming you are in the top-level project directory <code>cs1302-minesweeper-alpha</code>:The class path may be omitted in the first command if there are no other dependencies. Remember, when you compile <code>.java</code> files individually, there might be dependencies between the files. In such cases, the order in which you compile the code and whether or not you specify the class path matters.<strong>NOTE:</strong> If your grader needs to modify your directory structure or any of your filenames to compile your code, then the 10 point version of this penalty will apply. If, however, your grader is unable to compile your code, then the 100 point version of this penalty applies. Graders are instructed not to modify source code in an attempt to to make a submission compile.<strong>Any additional classes that you create should be located in or under the <code>cs1302.game</code> package.</strong> If other <code>.java</code> files are present, then they will be compiled <em>individually</em> by the graders and added to the class path, as needed, when compiling other files.</li>

   <li><strong>(100 points) Development Environment:</strong> This project must be implemented in Java 11, and it <em>must compile and run</em> correctly on Odin using the specific version of Java 11 that is setup according to the instructions provided by your instructor. Graders are instructed not to modify source code in an attempt to to make a submission compile.</li>

   <li><strong>(100 points) One Scanner for Standard Input:</strong> <a id="user-content-stdin"></a>Only one <code>Scanner</code> object for <code>System.in</code> (i.e., for standard input) should be created. <strong>You may create <code>Scanner</code> objects for other input sources as needed.</strong> Please note that if you create a new <code>Scanner</code> object at the beginning of a method or loop, then more than one object will be created if the method is called more than once or if the loop iterates more than once.</li>

  </ul></li>

</ul>

<ul>

 <li><strong>(0 points) [RECOMMENDED] No Static Variables:</strong> Use of static variables is not appropriate for this assignment. However, static constants are perfectly fine.</li>

</ul>

<ul>

 <li><a id="user-content-stdin"></a><a id="user-content-stdin"></a><strong>(20 points) Code Style Guidelines:</strong> You should be consistent with the style aspect of your code in order to promote readability. Every <code>.java</code> file that you include as part of your submission for this project must be in valid style as defined in the <a href="https://github.com/cs1302uga/cs1302-styleguide">CS1302 Code Style Guide</a>. All of the individual code style guidelines listed in that document are part of this single non-functional requirement. Like the other non-functional requirements, <strong>this requirement is all or nothing</strong>.<strong>NOTE:</strong> The <a href="https://github.com/cs1302uga/cs1302-styleguide">CS1302 Code Style Guide</a> includes instructions on how to use the <code>check1302</code> program to check your code for compliance on Odin.</li>

 <li><strong>In-line Documentation (10 points):</strong> Code blocks should be adequately documented using in-line comments. With in-line comments, you should explain tricky, large, complicated, or confusing blocks of code. This is especially necessary whenever a block of code is not immediately understood by a reader (e.g., yourself or the grader). You might also include information that someone reading your code would need to know but not someone using it (that is more appropriate for a Javadoc comment). A good heuristic for this: if you can imagine that, after six months, you might not be able to tell in under a few seconds what a code block is doing, then then you probably need to write some in-line comments.</li>

</ul>

<h2><a id="user-content-suggestions" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#suggestions" aria-hidden="true"></a>Suggestions</h2>

This project will be a lot easier if you structure your code properly. There is not a single correct way to do this, but here are some ideas for support methods that I think will make things easier. These are just suggestions. If you choose to use these, then you will need to implement them yourself.

The method above (as well as some other methods) can be implemented a lot more easily if you have an easy way to determine if a square is in bounds. Here is a suggestion for a method that does just that.

Also, it might be easier to use two different arrays (of the same size) in order to keep track of the game grid. One of the arrays could be a two-dimensional boolean array that indicates mine locations. The other array could be a two-dimensional char or String array that holds the blanks, numbers, and other characters for each square.

<h2><a id="user-content-how-to-download-the-project" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#how-to-download-the-project" aria-hidden="true"></a>How to Download the Project</h2>

On Odin, execute the following terminal command in order to download the project files into sub-directory within your present working directory:

This should create a directory called <code>cs1302-minesweeper-alpha</code> in your present working directory that contains the project files.

If any updates to the project files are announced by your instructor, you can merge those changes into your copy by changing into your project’s directory on Odin and issuing the following terminal command:

If you have any problems with any of these procedures, then please contact your instructor.

<h2><a id="user-content-submission-instructions" class="anchor" href="https://github.com/cs1302uga/cs1302-minesweeper-alpha#submission-instructions" aria-hidden="true"></a>Submission Instructions</h2>

You will submit your project via Odin. Before you submit, make sure that your project files are located in a directory called <code>cs1302-minesweeper-alpha</code> — if you followed the instructions provided earlier in this document to download the project, then your directory name should good. To submit, change into the parent of your project directory and execute the following command:

If there are style guide violations, then fix them and retest your code! Once you have no style guide violations and your code compiles, you can submit using the following command:

If you have any problems submitting your project then please email your instructor as soon as possible. However, emailing him about something like this the day or night the project is due is probably not the best idea.

5/5 - (1 vote)

<pre>score <span class="pl-k">=</span> <span class="pl-c1">100.0</span> <span class="pl-k">*</span> rows <span class="pl-k">*</span> cols <span class="pl-k">/</span> rounds;</pre>

<pre><code>        _  // (F)_ __   ___  _____      _____  ___ _ __   ___ _ __ /    | | '_  / _ / __  / / / _ / _  '_  / _  '__|/ //  | | | |  __/__ \ V  V /  __/  __/ |_) |  __/ |/    /_|_| |_|___||___/ _/_/ ___|___| .__/ ___|_|                             ALPHA EDITION |_| v2021.fa</code></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   | 1 |   |   |   |   |   | 2 |   |   |   |   |   | 3 |   |   |   |   |   | 4 |   |   |   |   |   |     0   1   2   3   4minesweeper-alpha:</code></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha:</code></pre>

<pre><code>minesweeper-alpha: helpminesweeper-alpha:         help</code></pre>

<pre><code>minesweeper-alpha: mark 0 0</code></pre>

<pre><code>minesweeper-alpha: mark 0 0minesweeper-alpha: mark     0  0minesweeper-alpha:     mark 0 0minesweeper-alpha:   mark     0  0</code></pre>

<pre><span class="pl-c">// Assume you have have a Scanner object that reads from System.in called stdIn</span><span class="pl-smi">String</span> fullCommand <span class="pl-k">=</span> stdIn<span class="pl-k">.</span>nextLine();  <span class="pl-c">// reads the full command from the user (Ex: command may contain "reveal 1 3")</span><span class="pl-c">// Create a new Scanner to parse the tokens from the given command</span><span class="pl-smi">Scanner</span> commandScan <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-smi">Scanner</span>(fullCommand); <span class="pl-c">// Neat! A new use of Scanner. :)</span><span class="pl-c">// Now, we can call our regular Scanner methods to get each part of the assigned command</span><span class="pl-smi">String</span> command <span class="pl-k">=</span> commandScan<span class="pl-k">.</span>next(); <span class="pl-c">// command would contain "reveal" from if given the command above.</span><span class="pl-c">// Continue to call additional Scanner methods (nextInt(), etc.) to parse out the other tokens from the full command.</span></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha: r 1 2 Rounds Completed: 1 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   | 2 |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha:</code></pre>

<pre><code> Oh no... You revealed a mine!  __ _  __ _ _ __ ___   ___    _____   _____ _ __ / _` |/ _` | '_ ` _  / _   / _   / / _  '__|| (_| | (_| | | | | | |  __/ | (_)  V /  __/ | __, |__,_|_| |_| |_|___|  ___/ _/ ___|_| |___/</code></pre>

<pre><code>$ echo $?</code></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha: m 1 2 Rounds Completed: 1 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   | F |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha:</code></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha: g 1 2 Rounds Completed: 1 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   | ? |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha:</code></pre>

<pre><code> Rounds Completed: 2 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |&lt;F&gt;|   |&lt; &gt;|   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha:</code></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha: hCommands Available... - Reveal: r/reveal row col -   Mark: m/mark   row col -  Guess: g/guess  row col -   Help: h/help -   Quit: q/quit Rounds Completed: 1 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha:</code></pre>

<pre><code> Rounds Completed: 0 0 |   |   |   |   |   |   |   |   |   |   | 1 |   |   |   |   |   |   |   |   |   |   | 2 |   |   |   |   |   |   |   |   |   |   | 3 |   |   |   |   |   |   |   |   |   |   | 4 |   |   |   |   |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |   | 6 |   |   |   |   |   |   |   |   |   |   | 7 |   |   |   |   |   |   |   |   |   |   | 8 |   |   |   |   |   |   |   |   |   |   | 9 |   |   |   |   |   |   |   |   |   |   |     0   1   2   3   4   5   6   7   8   9minesweeper-alpha: qQuitting the game...Bye!</code></pre>

<pre><code> ░░░░░░░░░▄░░░░░░░░░░░░░░▄░░░░ "So Doge" ░░░░░░░░▌▒█░░░░░░░░░░░▄▀▒▌░░░ ░░░░░░░░▌▒▒█░░░░░░░░▄▀▒▒▒▐░░░ "Such Score" ░░░░░░░▐▄▀▒▒▀▀▀▀▄▄▄▀▒▒▒▒▒▐░░░ ░░░░░▄▄▀▒░▒▒▒▒▒▒▒▒▒█▒▒▄█▒▐░░░ "Much Minesweeping" ░░░▄▀▒▒▒░░░▒▒▒░░░▒▒▒▀██▀▒▌░░░ ░░▐▒▒▒▄▄▒▒▒▒░░░▒▒▒▒▒▒▒▀▄▒▒▌░░ "Wow" ░░▌░░▌█▀▒▒▒▒▒▄▀█▄▒▒▒▒▒▒▒█▒▐░░ ░▐░░░▒▒▒▒▒▒▒▒▌██▀▒▒░░░▒▒▒▀▄▌░ ░▌░▒▄██▄▒▒▒▒▒▒▒▒▒░░░░░░▒▒▒▒▌░ ▀▒▀▐▄█▄█▌▄░▀▒▒░░░░░░░░░░▒▒▒▐░ ▐▒▒▐▀▐▀▒░▄▄▒▄▒▒▒▒▒▒░▒░▒░▒▒▒▒▌ ▐▒▒▒▀▀▄▄▒▒▒▄▒▒▒▒▒▒▒▒░▒░▒░▒▒▐░ ░▌▒▒▒▒▒▒▀▀▀▒▒▒▒▒▒░▒░▒░▒░▒▒▒▌░ ░▐▒▒▒▒▒▒▒▒▒▒▒▒▒▒░▒░▒░▒▒▄▒▒▐░░ ░░▀▄▒▒▒▒▒▒▒▒▒▒▒░▒░▒░▒▄▒▒▒▒▌░░ ░░░░▀▄▒▒▒▒▒▒▒▒▒▒▄▄▄▀▒▒▒▒▄▀░░░ CONGRATULATIONS! ░░░░░░▀▄▄▄▄▄▄▀▀▀▒▒▒▒▒▄▄▀░░░░░ YOU HAVE WON! ░░░░░░░░░▒▒▒▒▒▒▒▒▒▒▀▀░░░░░░░░ SCORE: 82.30</code></pre>

<pre><code>10 10 2 0 0 1 1</code></pre>

<pre><code>10 1020 01 1</code></pre>

<pre><code>    10    10 20       0 1      1</code></pre>

<pre><code>$ java -cp bin cs1302.game.MinesweeperDriver tests/seed1.txt</code></pre>

<pre><span class="pl-k">try</span> {  <span class="pl-smi">File</span> configFile <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-smi">File</span>(seedPath);  <span class="pl-smi">Scanner</span> configScanner <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-smi">Scanner</span>(configFile);  <span class="pl-c">// use Scanner here as usual</span>} <span class="pl-k">catch</span> (<span class="pl-smi">FileNotFoundException</span> e) {  <span class="pl-c">// handle the exception here</span>  <span class="pl-c">// and perhaps do the following for testing/debugging only:</span>  <span class="pl-c">// System.err.println(e);</span>  <span class="pl-c">// e.printStackTrace();</span>  <span class="pl-c">// and don't forget to:</span>  <span class="pl-c">// print any error messages described earlier and exit appropriately</span>} <span class="pl-c">// try</span></pre>

<pre><span class="pl-smi">System</span><span class="pl-k">.</span>err<span class="pl-k">.</span>println();<span class="pl-smi">System</span><span class="pl-k">.</span>err<span class="pl-k">.</span>println(errorMessage);<span class="pl-smi">System</span><span class="pl-k">.</span>exit(exitStatus);</pre>

<pre><code>$ minesweeper-oracle cs1302.game.MinesweeperDriver some/path/to/seed.txt &lt; optional/path/to/input.txt</code></pre>

<pre><span class="pl-k">private</span> <span class="pl-k">final</span> <span class="pl-smi">Scanner</span> stdIn; <span class="pl-c">// declare instance constant</span></pre>

<pre><span class="pl-c1">this</span><span class="pl-k">.</span>stdIn <span class="pl-k">=</span> stdIn; <span class="pl-c">// initialize instance constant</span></pre>

<pre><span class="pl-smi">Scanner</span> stdIn <span class="pl-k">=</span> <span class="pl-k">new</span> <span class="pl-smi">Scanner</span>(<span class="pl-smi">System</span><span class="pl-k">.</span>in);</pre>

<pre><code>$ javac -cp bin -d bin src/cs1302/game/MinesweeperGame.java$ javac -cp bin -d bin src/cs1302/game/MinesweeperDriver.java</code></pre>

<pre><span class="pl-c">/**</span><span class="pl-c"> * Returns the number of mines adjacent to the specified</span><span class="pl-c"> * square in the grid.</span><span class="pl-c"> *</span><span class="pl-c"> * <span class="pl-k">@param</span> row the row index of the square</span><span class="pl-c"> * <span class="pl-k">@param</span> col the column index of the square</span><span class="pl-c"> * <span class="pl-k">@return</span> the number of adjacent mines</span><span class="pl-c"> */</span><span class="pl-k">private</span> <span class="pl-k">int</span> getNumAdjMines(<span class="pl-k">int</span> row, <span class="pl-k">int</span> col) { }</pre>

<pre><span class="pl-c">/**</span><span class="pl-c"> * Indicates whether or not the square is in the game grid.</span><span class="pl-c"> *</span><span class="pl-c"> * <span class="pl-k">@param</span> row the row index of the square</span><span class="pl-c"> * <span class="pl-k">@param</span> col the column index of the square</span><span class="pl-c"> * <span class="pl-k">@return</span> true if the square is in the game grid; false otherwise</span><span class="pl-c"> */</span><span class="pl-k">private</span> <span class="pl-k">boolean</span> isInBounds(<span class="pl-k">int</span> row, <span class="pl-k">int</span> col) { }</pre>

<pre><code>$ git clone --depth 1 https://github.com/cs1302uga/cs1302-minesweeper-alpha.git</code></pre>

<pre><code>$ git pull</code></pre>

<pre><code>$ check1302 cs1302-minesweeper-alpha</code></pre>