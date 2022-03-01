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
        // u may go doc to set as how official defined, here i only wanna mention idea, concept
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
    ~/foo.txt    gf(file) can't 3gf? or select multi path to open multi files???
    
    gh   gs                  gl where is go char: /n? as /n is leftmost char??? new key or f /n?
    
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

sometimes, keymap doc is the final result, like 2-base program according to source code,
 u may read it, u may puzzled it
cuz u r human, and how program became/evolve to final format, this is unknew to u.

u don't know why it is like this or that, what pros&&cons, which better. what u read is the final
result to use, of couse, u can just remember it, but that is ez to forget. 
at least for me, i will forget the unreasonable things.so i have to find out why...

plus, sometimes u don't understand source code, cuz the source code is not that source. it is some
level of 2-base program, the original thinking and processing is in developer's brain,
and the genius "source code" is not the "source code" u see in github, it is in brains of architechture
of who build them. so you need reverse the final result, figure out why it became like this.

c-u c-d half screen up/down
    i wonder why this can'd count? e.g. 2 c-u? though it is merely usage
    where is go last cursor position???or cancel the selection current made, back to last selection?
    ctrl-. also works on adding? not deleting repeat?
c-i c-o, jump in, out (2 direction of 1D jump list)
    in powershell, c-i didn't work, may confilict with others key bindings..

v     visual select mode
g
m    match mode
:     run command mode
z    view mode
Z    sticky z???
     space mode

in normal brain, what human know, is that, i see apple, and i'm thinking, i wanna eat it!
so, why helix, choose 2wd, not d2w? when we see text on screen, we have already have the concept
where to delete, so just direct delete them!see? delete them! not them delete!!!! 
this design made me no freedom of what exist in my brain. which is, i know, you know, some language
design like i apple eat format, but not mainly language, let's face the fact, english is the first-class
supported in the world, and helix don't support? it's like qwerty vs colemak, may colemak sounds or tested
better to new users, but for a man who have used qwerty, colemak is not a better choice. the same, in our
brain, there do has a delete 2 words pattern, and the software don't help me to finish it, it ask me to
do the translating job, fk my brain? who work for who?

C-s add cursor to jump list?? then why just letting me to jump cursor??why definition of jump list exclude
    cursor position?

i know i know, when i know part of helix, some my thinking is not that correct, cuz for a complex staff,
u'll always have puzzle part. may helix handle it in a nother model better, but from now, i have to
rely on these questions, may fault in future when i learned more on helix, but it is neccesary.

why u choose this over that, cuz it powered u, u became more powerful, more freedom to control the world.

but the helix is too young at this moment, but obviously, its key mapping and fn are much better than
default of vim or emacs. but at now, vim/emacs have plugins, which i didn't see in helix...

only few config i can free define. not that much free. e.g. define d2w d2b, since in helix w don't mean word
but forward by a word, b mean backward by a word, h mean backward by a char, not left.
when model has exception, e.g. u think h is left, u lack of other model such like measure word: cell/word/line
, h is not only left, but forward a cell.w is not word, it's forward a word, and impl is to the word end or
if with a space, included.

see, it it not single element, it's combination of many meaning.

every choice has cons and pros, helix choose consistancy, but not to the most usage style, consistant to
a second syntax, which puzzled me. is the author born in a country that treat En/US as enemy?
and ur language is  i-apple-eat style? as for my countries, China, the old language in history has the
part like i-apple-eat style, but modern language is common i eat apple style since 1911 approximately.

but since custom config emacs works like helix may pay too much time, i would choose to tolerate
this design of 2wd of helix at the moment. or maybe when possible, branch helix or add a plugin?

see, i-apple-eat or i-eat-apple or apple-eat-i or apple-i-eat or eat-apple-i or eat-i-apple
are all same level ez-to-understand to computer, but for a man who used to i-eat-apple,
i-eat-apple is a better choice, not cuz of it's better, but it's better for me
it's better for me not cuz of it's better, cuz of i've choosen it, then it is better.

changes
r
    replace selection to same 1 char
        // how weird it is...
    if u select 1 char, then normal replace
    if u select multi char, then change all selection to same 1 char
    again, can't 2r, which is fk not good enough!
    what if u wanna replace one by one?
        no way...use change to broke the screen representation of text
        vlr? nope, this changed all to same one
        vlc?, god, it worked, but ur brain will need to remember u need type 2 chars.
            which should letting computer promp u to do the job!!!!!!!
R
    worked like paste in GUI, u choose something, then replace with copyed content
    so it's like pP, not R
~
    upper-case lower-case switch {a, A} -> {A, a}
`
    to lower, {A} -> {a}
A-`
    to upper, {a} -> {A}

where is Ctrl-`?, since this key is about upper/lower case
    how about set this to a format: 
        {applePrice, ApplePrice, apple-price...} -> {apple_price} 
        rust style? sounds good huh?
i
    insert before selection
        it's different from vim to insert to cursor cell
    so if u wanna change on cursor cell after a w, u need ; to cancel selection u made
        and it turn to default min selection -> 1 cell
a
    after
I
    xi VS shift-i?
A
    xa VS shift-a?
o
    new line after selection, then inser
    how about add an empty line and just stay in normal mode?
        ctrl-o?    why not just goto mode for go jump-list cursor-list?
                            hyper-link? and cursor-list?
                                what 99gg f; ? they are corsor move or jump move?
        alt-o?
O
.
    repeat last change??
        change???
            add?
            delete? why delete with its selection is not a change?
            replace? r is not treat as change???
        change only include in insert mode?
            abc add
            backspace
        what about other mode repeat?
u/U
    undo/redo
        why undo treat delete replace as change? not consistant with .
a-u/a-U
    real undo/redo, not single branch of changes, but all changes as an array, back or forward
y
    where is Y
    sd
p/P
cursor     paste b/a selection
    yank a word
        paste on line before? (actrually , no need of this goal while coding...)
                xPa enter?????? so fk complex?
                O,ESC,h,p
                O,ESC,P?
    selection position
        if cursor is on start of a line, use P to paste before line
        if cursor is on end of a line, use p to paste to create next line
    yank a line
        just pP
"
    clipboard index, choose which to copy/paste
><
    finally, 2> worked.....
        tell me, u give in the usage? > is just a verb, and can add count before it???
        finally not consistant with [count] selection verb???
        go impl my d2w pattern!
=
    format 
d
    it's a cut(Ctrl-x) in windows,  though u can use cut to delete
    delete selection, and in 1 line, latter chars will auto move to left
    why can't backspace work??? in normal mode???
    also ctrl-h not work in normal mode??? 
        if code too many years, u will like Ctrl-h
        it won't fold ur hand wrist in far-away backspace, less pain
            if ur little finger pain
            remap you space's tap-state to space while held-state to ctrl
            or buy kinesis advantage 2!!!
a-d
    pure delete(detele), no auto copy with delete content
    useful when u wanna paste something while need to delete something first.
c
    change-selection
     === di
a-c
    c without copy
c-a/c-s
    puzzled me, what's this?
    object ? word? line? where?
Q
    macro Q star/stop
    "aQebc, ESC, Q     ===  
    how to cancel macro to store in a register
    how to stay in insert mode? use A-Q for stop macro?
q
    macro call()
    "aq
    maw can handle more general cases.
        eg or ge, sometimes eg's e will go next word if cursor on end of a word
use the most useful features, leave out the others, or u loose all if u wanna all

thus, features or keys are in one list, but they are not in same-level importance.

if a feature the developers didn't even fix, let it be, perfect is shit.

|
    replace selection by command output
    or call(selection) ?
    showed failed in powershell
list:
    pwd
    www.baidu.com
a-| without output
! insert before
a-! append
$ filter only return 0

s
    regext filter
    sub selection
S
    split
    divide
a-s
    split in new line
&
    align
_
    to deselect spaces!!! we! maw!
;
    cancel selection
a-;
    reverse cursor from target to base
,
    delete all other cursor
a-,
    delete 1 cursor
C
    multi selection on next line of current selection
    e.g. wCC, well, not semantic, only same position on 2D screen
    support w3C
a-C
    before
()
    puzzled me, i tried C or w to move cursor, not this usage, then for what?
    ok, work in s mode, to choose one, 
    but screen can't show difference as they all hlted, but corner column number changed
a-( a-)
    i don't need to figure it out..
    let it be there
%
    select entire file
    god, ggvge not select last line's other chars....
    and where is my V?
x
    whole current ! line !, include /n
X
    puzzle me..o, yeah, when multi selection, it works like x
    when after C, () to choose one line, x to select whole current line
    if u wanna everyline, X 
    
    why not just let X as choose backward next line???????
    who need this difference of current line in multi cursor mode????
        thus why X is X, not a backward x?
J
    can't support 2J
    use xx then J
K
    puzzled me. since we have s and enter, why not use s again nest in s?
    what's purpose of K? keep what? don't s keep the selection by default?

    
    what's purpose of K? keep what? don't s keep the selection by default?
    hello helle helloa
a-K
    doesn't work as i thought it won't just hlt like s, it will remove
c-c
    comment switch
a-k
    extend to parent node
    vgld???????
        not d$????
a-j
    shrink
a-h/a-l
    sibling note
/ ?
    n N
    * selection -> register \
        e.g.     /\
z view mode
    zz === zc waste?? for consistency
    zt/c/b    like gt/c/b not go screen's line t/b/c, but let the curent line to be on screen
        when you wanna change content, go g/t/b/c, when u wanna read detail about current line
        zt/c/b
    zm
        like c in another 1D(horizontal) of 2D screen
    zj/zk
        fk not consistency with model zt/c/b, not operate on currentline, operate screen up/down???
        
        



    





            
i'm not an object-lover, i'm a features-lover. if one feature is good for me, i love it.
we all know object has cons and pros. but features are pure cons or pros. haha. cuz u
get one object, u get all cons and pros. but if u get a feature, if it has cons, just
difine a new feature to point to the pros...haha

so once there are good staff in helix, i love them, but i will hate bad staff in helix.
so to helix, there is no love or hate, to helix' developers, i always thanks them, they help me
to wonder such an new editor, an editor better then default of vim/emacs.

        
m
    maw selects spaces if only one word in a line???
        wtf, it also match a word if has latter space or former space????
        use be or eb
    since eb is better then maw, why not keep maw to other most useful binding?
    

        
```

8a
bbb aac
asd aaa8
aaaaah iii j
1
2
3
4

    
lskdjflskjaddfffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffflksjlkjfalskjdflksjlkfdjalsjdfk
    lkj ksljdflkjs
