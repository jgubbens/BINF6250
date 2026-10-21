# Introduction


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
* After successfully running our program, we tested it on different sequences to make sure it worked beyond the two given examples. 

# Struggles
* We were initially not sure how to handle equally optimal alignments. If multiple cells share the maximum in the matrix, traceback starts from the first one encountered. Our code also prioritizes diagonal moves over gaps when scores are tied. 

# Personal reflections
## Group Leader: Justin Gubbens


## Other members
### Selin Uenal
This project truly felt like a breath of fresh air after the last two. In those projects, while I understood the concepts, I found it hard to translate them into actual code. This project was a lot of fun because the concept was interesting and made sense, and the code was intuitive without feeling too far outside my comfort zone. Our group worked very well together this week, spending a few hours on a Teams call over the weekend pair programming. We were able to finish the majority of the project together, synchronously, which isn't always possible. I also learned a lot from my group members and their different areas of expertise, and feel confident in my abilities to implement the Smith-Waterman algorithm. 

### Yulia Murthy


# Generative AI Appendix
Claude (Sonnet 5.5) was used to make sure we kept our traceback direction definitions consistent throughout our program. We fed the program our final code, and asked "Are the traceback directions and string labels in the Smith-Waterman implementation consistent throughout?" It let us know that we did keep our definitions consitent, but one of the docstrings did not match our traceback directions, so we were able to update that. 