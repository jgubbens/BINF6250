# Introduction
This project implements the Smith-Waterman algorithm for localized sequence alignment. As a localized algorithm, Smith-Waterman finds the highest-scoring matching region between two sequences without requiring end-to-end matching. This approach uses dynamic programming to break the problem up into small pieces in the form of a scoring graph, allowing for problems to be solved using iterative small steps that can be repeated to solve long sequences.

# Pseudocode
```
FUNCTION cal_score:
    IF first row or column:
        RETURN 0, END

    Calculate diagonal score using match or mismatch
    Calculate up and left scores using gap penalty
    Select highest score from diagonal, up, left, and 0

    RETURN score and direction

FUNCTION traceback:
    INIT empty aligned sequences
    Start at highest-scoring position

    WHILE direction is not END:
        IF diag:
            Add letter to the beginning of both strings
            Move diagonally
        ELSE IF Up:
            Add seq1 letter and gap in seq2 to the beginning of strings
            Move up
        ELSE IF Left:
            Add gap in seq1 and letter in seq2 to the beginning of strings
            Move left

        UPDATE direction

    RETURN aligned sequences

FUNCTION smith_waterman:
    INIT scoring and traceback matrices with zeros
    INIT maximum score = 0 and maximum position = (0, 0)

    FOR each cell:
        CALL cal_score
        STORE score in scoring matrix
        STORE direction in traceback matrix

        IF score is greater than maximum score:
            Update maximum score and position

    CALL traceback from maximum position

    RETURN aligned sequences and scoring matrix
```

# Successes
* Our outputs from the examples given exactly matched the expected results on the second run.
* After successfully running our program, we tested it on different sequences to make sure it worked beyond the two given examples.
* We used the scoring matrix from class to visually keep our ideas in check with the potential flow of an alignment. 

# Struggles
* We were initially not sure how to handle equally optimal alignments. If multiple cells share the maximum in the matrix, traceback starts from the first one encountered. Our code also prioritizes diagonal moves over gaps when scores are tied. 
* In our first run of code on the `seq1` and `seq2` strands, we had gotten the output below:
```
TAC-TTA
CACATTA
[[0. 0. 0. 0. 0. 0. 0. 0. 0.]
[0. 0. 0. 0. 0. 1. 1. 0. 0.]
[0. 0. 1. 0. 1. 0. 0. 2. 1.]
[0. 1. 0. 2. 1. 0. 0. 1. 1.]
[0. 0. 0. 1. 1. 2. 1. 0. 0.]
[0. 0. 0. 0. 0. 2. 3. 2. 1.]
[0. 0. 1. 0. 1. 1. 2. 4. 3.]
[0. 0. 0. 0. 0. 0. 1. 3. 3.]]
```
This output had identical scoring matrix as the expected, but in our traceback our code prioritized the diagonal moves when scores were tied at the end position. At the (1,1) position, the traceback should have ended there since the cell score was 0, but (1,1) held the T-C mismatch and as scores from all directions were 0, it prioritized the diagonal. It was then hard-coded to end at (0,0). We had to edit the first function to include `scores[0] = 0` to create that end state, which then produced the expected output for us. 
* We made another fix after successful runs; when running our code through Google and Claude we encountered a potential bug that we did not catch previously. Initially, our code returned non-end state moves for the borders:
```
if i == 0 and j != 0:
    return 0, 2   # UP
if i != 0 and j == 0:
    return 0, 3   # LEFT
```
We then changed the code to create the end states for the borders with `if i == 0 or j == 0: return 0, 0`


# Personal reflections
## Group Leader: Justin Gubbens
This project was relatively straightforward for our group. While the past couple of projects were conceptually difficult and took a lot of clever coding to complete, our group was able to finish almost the entire project within just one synchronous meeting. I believe this was because we all came into the project already fully conceptually understanding the algorithm and what needed to be translated into code. When it came to actually writing the code, we took it on as a group effort, deciding how the methods should be organized and writing code syntax on a shared screen. While there was initially some confusion regarding order of operations in the traceback method, we were able to stop and conceptually run through an example from a picture of a scoring matrix together to figure it out. Overall, I feel that we all came out of the project fully understanding the concept of the algorithm and the code that runs it.

## Other members
### Selin Uenal
This project truly felt like a breath of fresh air after the last two. In those projects, while I understood the concepts, I found it hard to translate them into actual code. This project was a lot of fun because the concept was interesting and made sense, and the code was intuitive without feeling too far outside my comfort zone. Our group worked very well together this week, spending a few hours on a Teams call over the weekend pair programming. We were able to finish the majority of the project together, synchronously, which isn't always possible. I also learned a lot from my group members and their different areas of expertise, and feel confident in my abilities to implement the Smith-Waterman algorithm. 

### Yulia Murthy
I enjoyed this project, it felt like the most collaborative session I've had in a group. We met up for a Teams call, and while I had shared my screen with the Jupyter notebook, the three of us worked through the code ideas and the flow of the functions step-by-step. We were able to finish the project in just under 3 hours! I definitely benefited from being the one to share my screen, as I did not have as much programming experience as Justin and Selin, it helped for me to be the one writing the code even if there were sections I was more stuck with than the others were. In this process, I helped gain a better understanding of how certain code modifications can alter the output. It also helped for me and my group members to refer to the visual of the scoring matrix throughout this process. I tend to have a harder time understanding concepts if there is no visual, so in times when I felt confused, my group members were able to use the visual to help me understand how the code finds the alignment. Overall I felt we all had excellent ideas, and were able to brainstorm with a similar level of understanding. I think a future step for this project would be to handle cases where more than one alignment possibility can occur, as we have seen in the class examples. 


# Generative AI Appendix
Claude (Sonnet 5.5) was used to make sure we kept our traceback direction definitions consistent throughout our program. We fed the program our final code, and asked "Are the traceback directions and string labels in the Smith-Waterman implementation consistent throughout?" It let us know that we did keep our definitions consitent, but one of the docstrings did not match our traceback directions, so we were able to update that. 
