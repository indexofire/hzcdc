# Genome Assembly

![Genome Assembly](../assets/images/2.1/banner.jpg)

因为微生物一般是单倍体，基因组较小，重组频繁，所以常采用基因组测序了解其遗传信息。


序列拼接方法：

  - OLC(Overlap Layout Consensus):
  - DBG(De Bruijn Graph):
  - String:

```mermaid
    gantt
    dateFormat  YYYY-MM-DD
    title Adding GANTT diagram functionality to mermaid

    section A section
    Completed task            :done,    des1, 2014-01-06,2014-01-08
    Active task               :active,  des2, 2014-01-09, 3d
    Future task               :         des3, after des2, 5d
    Future task2              :         des4, after des3, 5d

    section Critical tasks
    Completed task in the critical line :crit, done, 2014-01-06,24h
    Implement parser and jison          :crit, done, after des1, 2d
    Create tests for parser             :crit, active, 3d
    Future task in critical line        :crit, 5d
    Create tests for renderer           :2d
    Add to mermaid                      :1d

    section Documentation
    Describe gantt syntax               :active, a1, after des1, 3d
    Add gantt diagram to demo page      :after a1  , 20h
    Add another diagram to demo page    :doc1, after a1  , 48h

    section Last section
    Describe gantt syntax               :after doc1, 3d
    Add gantt diagram to demo page      :20h
    Add another diagram to demo page    :48h
```
```mermaid
graph LR
A[Square Rect] -- Link text --> B((Circle))
A --> C(Round Rect)
B --> D{Rhombus}
C --> D
```

```mermaid
graph TB
sq[Square shape] --> ci((Circle shape))

subgraph A subgraph
    od>Odd shape]-- Two line<br>edge comment --> ro
    di{Diamond with <br/> line break} -.-> ro(Rounded<br>square<br>shape)
    di==>ro2(Rounded square shape)
end

%% Notice that no text in shape are added here instead that is appended further down
e --> od3>Really long text with linebreak<br>in an Odd shape]

%% Comments after double percent signs
e((Inner / circle<br>and some odd <br>special characters)) --> f(,.?!+-*ز)

cyr[Cyrillic]-->cyr2((Circle shape Начало));

 classDef green fill:#9f6,stroke:#333,stroke-width:2px;
 classDef orange fill:#f96,stroke:#333,stroke-width:4px;
 class sq,e green
 class di orange
```

```mermaid
sequenceDiagram
Alice ->> Bob: Hello Bob, how are you?
Bob-->>John: How about you John?
Bob--x Alice: I am good thanks!
Bob-x John: I am good thanks!
Note right of John: Bob thinks a long<br/>long time, so long<br/>that the text does<br/>not fit on a row.

Bob-->Alice: Checking with John...
Alice->John: Yes... John, how are you?
```

```mermaid
sequenceDiagram
loop Daily query
    Alice->>Bob: Hello Bob, how are you?
    alt is sick
        Bob->>Alice: Not so good :(
    else is well
        Bob->>Alice: Feeling fresh like a daisy
    end

    opt Extra response
        Bob->>Alice: Thanks for asking
    end
end
```

```flow
st=>start: Start:>http://www.google.com[blank]
e=>end:>http://www.google.com
op1=>operation: My Operation
sub1=>subroutine: My Subroutine
cond=>condition: Yes
or No?:>http://www.google.com
io=>inputoutput: catch something...

st->op1->cond
cond(yes)->io->e
cond(no)->sub1(right)->op1
```

## Reference

1. [Homologs De Bruijn Assembly Tutorial](http://homolog.us/Tutorials/index.php?p=1.1&s=1)
2. [EMBL-EBI Training: De Bruijn Assembly]
3. [Ben Langmead's De Bruijn Graph Assembly](http://www.cs.jhu.edu/~langmea/resources/lecture_notes.assembly_dbg.pdf)
