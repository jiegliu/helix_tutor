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
        some codes may invisible: space, enter, tab...
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
    4. the cursor pointed to the leftest char of line
    5. the cursor pointed to a fn
    6. the cursor pointed to a line 18
    7. the cursor pointed to a char named "f"
    8. the cursor pointed to a fn which has a debug info
    9. the cursor pointed to a line below the comment where i wanna replace the info!
    ...

it all depends on how u think of it, what u need, which model is more ez or efficient
    to handle ur job.

move

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
            need work with another 1D(horizontal) to make the whole text.
!!
    set "line-number = "relative"" to moving in lines
    rather 1024gg, now we can handle lines only in screen use relative line number
        we operate on screen lines, why start counting at invisible logical text lines!
if import whole mode of every dimension's directions of 1D and 2D, will need more keys.
    so helix use hljk as 4 common sharing part of basic, which is not full, but pay less, take more
which cause a problem, exceptions corner-case...e.g. horizontal move leftest/rightest is not consistant
with jk as hl when meet ends. so u will not use hhhhh to meet leftest char, but this is rare usage.
 we can use another model to produce this result, not consistant but first thing first,
so lefest is gh(chars of text) or gs(visible chars of text), instead of hhhhhh
better solution, though breaking the consistancy.perfect is shit. we only wanna pay less to produce more

wbe

hl is not mainly on left or right meaning, it is marked more on measure word 1 cell.
    so 8l is rightward 8 cell
w is also right(forward) a word start, it is marked more on measure word 1 word.
    so 8w is rightward 8 words

when we use w, there are 1 info discarded in vim, cursor and the selection from origin cursor to new curso
which is kept in helix, good design, i just egc change the word, vim need key wce ...
but if just deleting, better on helix, one key less then vim. but more brain calculating on space..

why not 2 w is go on right 2nd word, select the word???
cuz of current is the 1st word, ugly impl

when with spaces
    when cursor in a word
        w e dosen't mainly mean word start or end, 
            they both select from current cursor to the current word end(functional programming! brain-friendly)
                which is that i wanna thinking in word not index or even 2-base data!! 
                they are same on value, but not a brain-oriented data format.
        e stay cursor on word end
        w stay cursor on word end then go more with a space
    when cursor not in a word
        e moves with former space(if there has == if not a char start of line)
        w moves with latter space(if there has == if not a char end of line)
when without spaces
    w are fk same with e!
b is like mirror of w..
    
for plus info between foo bar lar!
    wi, wa, ei, ea, wd, ed
    w;d, wA-;

use w with selecting latter space, use e with selecting former space 

and here are ugly things, what human need to jump to next word, but it use index-impl rather 
then human-brain-oriented-impl, it fk not jump to select next word but fk select a space over it.

it's convenient for deleting when we auto has a space, but not for replacing.
so go use e eee and then g back to choose the word! aha, ur welcome.
actually there are too many needs on moving a word, so we are basic two, plus eg, satisfy main usage.
not that concise or brain-friendly to define word, word with space before and latter. 
if so, need 3 keys on forward a word. but isn't it a w e eg??? just not that obvious
so if these models are better to use , why use fking move index to next word start char!!
these descriptions are shit that fk the brain, but letting itself simple, not simple for brain
emacs use select space between word, if wanna replace or delete, need more like dw rw?
sounds better...helix has pros on default e , it satisfy most need, then we need d or c is enough.
not consistant, but enough to use...

use e as possible as u can, when delete a word
(why space existed? it is usually typed for the latter word) so why u use w?
and when replace a word, ebc is better than wc with xxx then add a space back

w is for gh then v3wd?

fk stop blinking on black pop window, hot to turn prompt off???

e; for edit last char of word
wA-; for edit first char of word
w if delete spaces if a line start with many spaces
3e to jump approximately position, 3 is near e, ez to tap

WBE
    same usage of web(most with space part), cuz of there is no non-space parts..
        it only stop at "space"....
tTfF
    single char search then move work in 1D codes
        not like vim in 1D(horizontal line) of 2D screen
    how to repeat???
        shit A-.????????????? why not just one key?can work with multi-cursor and multi selection?
            so can't use , and ;?  and work with .???
        f"ec to change "hello"
            it's different with 2f", whose current cursor changed, selection changed
        2t"T" to change "hello world ahaha, slkjflk"
            f"lt" seems better..
        it's better select with a reverse order, start from the forwardest, then TF to backward
            cuz of noise is in begining position, while in the end, their only ""));
                if lucky only )
    it also has a multi-line selection.hmmm, powerful
    also x can't work with C? but X is fk what meaning works like x in C???
G
    i always think that G is a waste since there already exist a gg
        which is beter then G: Shift-g, move two fingers, type and wait and release
    i'll remap it into other staff
so far upper-case HL is free for users to define
    J for join, can't 2J???? fk stupid design. what is Join, Join is a line operator, of course
        it repeat means multi line! it is not a delete or replace who has no selection information
    K can't figure out what's meaning of it
        sub select? what is keep???why not use select to select?
tell me, why 88gg not with selection? need add v? while others move has selection???
g, go mode
    ga last accessed file
    gm last modified file
    
    buffer
        gn
        gp
    g.
        last modification position of this file
    gg
    
                gt
    ~/foo.txt    gf(file)
    
    gh   gs                  gl 
    
                gc
                
                bar()         gd(definition)
                
                sum_of_apples gy(y, t, ai, type definition) ???
                
                &foo          gr(reference)
                
                struct        gi(impl)
                gb
    
    ge(end)
    

well, i would let u/d upmost instead of top, so we can use type definition:gt
or why not gT with gt?
still there exist many empty key, which is good to define user's key

A-: make sure forward selection?
how this matters? what usage? there only FTB with backward selection, so what? make forward?
and then what?since there is A-; to reverse select?

only difference is that A-; is a switch, may cause different result

but A-: will only make one result: in two chars, may u don't see screen clearly which is selected,
so u use A-: to make sure u choose the forwardmost one, or then A-; to reverse it...

why not define this in ctrl-;! why shift-alt-;!

c-u c-d half screen up/down
c-i c-o, jump in, out (2 direction of 1D jump list)
    in powershell, c-i didn't work, may confilict with others..

v     visual select mode
g
m    match mode
:     run command mode
z    view mode
Z    sticky z???
     space mode
    
        
```

aa
bbb ccc
ddd eee fff
ggg hhh iii jjj

delete
    
lskdjflskjaddfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffflksjlkjfalskjdflksjlkfdjalsjdfk
    lkj ksljdflkjs
