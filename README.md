# helix_tutor

```

welcome.

one info may have different format for different usage:

we list some text models what a user need, not all but enough for users.
    1. ez for man to understand via brain <- 2D logical text
    2. ez for man to read on screen by eye via brain  <- 2D screen text: partial 2D logical text
        many lines may be out of screen(text has too many rows/lines)
        also many columns may be out of screen(rows are too long)
    3. storage in computer <- 1D asciis or unicodes format codes
        some codes may
    4. for show(to be) <- screen cells, may occupied by codes, or nothing
cursor: select one cell(code) on screen

one thing/staff/object may have different aspects/descriptions/explanations:

 3     x*x
 2 }
 1 // author: foo
18 fn add_bug(){
 1     println!("stack over flow");
 2 }
 3 fn del_bug(){
 4     println!("");    // no msg is best msg
    
    1. the cursor pointed to a cell of 2D screen
    2. the cursor pointed to a code of 1D codes
    3. the cursor pointed to a start char of word
    4. the cursor pointed to a start char of line
    5. the cursor pointed to a start char of fn key word
    6. the cursor pointed to a start char of line 18
    7. the cursor pointed to a start char named "f"
    8. the cursor pointed to a start char of a fn which has a debug info
    9. the cursor pointed to a line below the comment where i wanna replace the info!
    ...

it all depends on how u think of it, what u need, which model is more ez or efficient
    to handle ur job.

selection

h/l
    is
        1D codes(unicodes or ascii stream) backward/forward by 1 code
            1. will move except when cursor is on first/last code.
            2. if moved, select the cursor cell
            3. for 1D codes, like an array, has leftest end and rightest end
                (don't wonder there is no leftest in dictionary, we are creating history!)
                so in combination with other keys, h/l means leftest or rightest.
    is not
        screen cell left, though in 1 line, they are same.
        what are they? 1D move and 2D move, 1D codes >= 2D screen text or 2D logical text
        so some time, they may act same, horizontal dimension of 2D.
??
    why there is no config of showing relative columns number?
        so ez to 8h to a particular position
    what other better models for moving in line?
j/k
    is
        1D(vertical dimension of 2D logical text) backward/forward by 1 code
            1. will move except when cursor i on top/bottom row/line
                    (the 2 ends of the dimension)
            2. some row may have different width of row, from long row end to downward
                cursor only select text, will not stay on empty cell of screen
                will stay on nearest occupied cell of text
                
    is not
        1D codes, though they are both 1D, but one is full 1D, one is sub dimension of 2D,
            need work with another 1D to make the whole text.
!!
    set "line-number = "relative"" to moving in lines
    rather 1024gg, now we can handle lines only in screen use relative line number
        we operate on screen lines, why start counting at invisible logical text lines!
        
```


delete
    
    lskdjflskjaddfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffflksjlkjfalskjdflksjlkfdjalsjdfk
    lkj ksljdflkjs
