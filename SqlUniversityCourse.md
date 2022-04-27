CREATE TABLE Students(id int, name text, Gpa real)
CREATE TABLE Courses(id int, name text)
CREATE TABLE Enrollment(studentId int, courseId int)

//I need to add constraints about fields that shouldn't be null
//ALTER COLUMN Students(id) IS NOT NULL

ALTER TABLE Students ADD PRIMARY KEY(id)

ALTER TABLE Enrollment ADD PRIMARY KEY(studentId, courseId)
ALTER TABLE Enrollment ADD CONSTRAINT FOREING KEY(FK_Enrollment_Student_id)
ALTER TABLE Enrollment ADD CONSTRAINT FOREING KEY(FK_Enrollment_Courses_id)
REFERENCES Students(id)

ALTER TABLE Courses ADD PRIMARY KEY(id)


//JOINS
1) Simplify with USING 
    SELECT *
    FROM TABLE A
    JOIN TABLE B USING (id)
2) Use natural join for fields with the same name //But it's not reccomended in
practical use
    SELECT *
    FROM TABLE A
    NATURAL JOIN TABLE B 
3)

//Group
    1) When grouping by you do next
        - First - defining sourses by From clause
        - then filter it by where clause
        - then group it by GROUP clause. Grouping works like that:
            accouding to defined columns the system group each values in separated
            groups according their equality. So if all values are equal it combine them
            into one group and setera. 
        - Then it apply functions from Select clause 
    2) Group by means that in the result will be one line result for each group,
    that is why aggregates asks for group by when select clause contains
    additionaly to aggregate function columns
    3) So results represented with using group clause means that each row of
    result is the result for each group separatelly.
    4) If I like to apply some filtering after grouping I need to use HAVING
    clause.

// Additional clauses
    1) Except - shows data which is not equal between queries
    2) Intersect - shows data which is equal between queries
    3) union - shows unioned data 

// Correlated and uncorellated subqueries are queries that matching with the
main query by some variables from parent query or not accordingly.

// Table schema
    - table schema (constraints info, column names, types) are stored in database
        catalog. Database catalog is persisted in repository of DB where stored
        metadata about all data that are stored.
    - Actual data of a table (content) is stored as a collection of Pages
      (file). It has a size which is optimized for current storage device to
      retrieve a chank information. Again all pages that contain table data is
      called as File.
    - SLOTS contain information about data within row and it's tored inside page
    - RECORDID its a pageId + slotId
    - Packed representation - it's the way to store slots when we keep all
      actual or in use data close like:
        used
        used
        used
        free
        free
        3   <- count of used slots
      But the problem is what if we delete the second slot, then system should
      shift all record up. Additionaly system shoul recalculate RecordID. (It
      lools like how managed heap is designed. GC packs heap and recalculate
      references.:w
    - Unpacked is alternative to packed when we store slots with free mixed
      like:
        used
        free
        used
        free
        free
        10100 <- map to describe page

//Indexes
    - index stores references as PageId and SlotId to table row.
    - index group column values, so it looks like key i.e creating indexes it's
      the same as to create keys. (I found it looks similiar to virtual memory
      organization, like each process has it's own address space which started
      from 0 to it maximum and each segment reffers to physical memory address
      which is could be in RAM or on SWAP file.)
    - FLAT INDEX means that we have a just collection references to rows.
    - TREE INDEX means we have some sort of structure that keeps additional
      information that helps do binary search more efficient by using additional
      leafs which holds mettadata representing in the same way as FLAT INDEX but
      now it has layers like we divide the final root by some parts and keep
      that information in leafs and that leafs we divide on more parts and
      create one more layer with leafs that keep the same structure as FLAT
      INDEX pattern.
    - So root FLAT INDEX of a INDEX TREE holds references to data, when leafs
      holds references to ROOT FLAT INDEX PAGES.
    - USE: When quality predicate like SOMETHING = TO SOMETHING or inequality
      like SOMETHING <= TO SOMETHING
    - QUALITY search => just to get data from founded leaf reference.
    - INEQUALITY search => fo that we need to get more data from different pages
      so in that case more efficient will be if each root pages have link to
      previous and next page. (SO if I want to implement search engine some
      where so better will be to use already existing practice like mentioned
      here)
    - It matters when we write queries because having special index like
      composite index we can write query with wrong order wich break our
      performance.
    - COMPOSITE INDEX the order is matter. It will be unefficient if my query
      will have WHERE clause with using ONE key and this KEY has the second
      order in composite index. like for instance i have composite key (id, name)
      and searching by using both will be ok, searching by using id will be ok,
      but using just name will be unefficient.
    - CREATE INDEX SOME_NAME ON TABLE_NAME (COLUMNS)
    - MANY INDEXES is overhead when table is updated frequently and so system
      should update all indexes.
    - CLUSTERED INDEXES are tables )) (EFFICIENT comparable to NONCLUSTERED
    - NONCLUSTERED INDEXES are additional files that keep references.
      UNEFFICIENT comparable to CLUSTERED
    - Tree indexes based on sort order which means the similiar indexes are
      close each other. So we can think that probably values the similiar too or
      close each other.
// Hash indexes
    - based on hash values. So if the similiar key was found it doesn't mean
      that they have the same values.
    - Hash table stored in memory, like dictionary contains key and value, where
      value is a bite representation of position in file.
    Hash vs btree need to cover in detail soon.

    Buffer manager
    - works like RAM and virtual memory.
    - it has a bufer pool wich is like RAM. Buffer manager accept requests and
      check bufer. Like Ram manager bufer decides what frames to erase and what
      to load (Replacement policy).
    - Why we use additional component like buffer manager instead virtual memory
      it's because dbms has specific patterns to access data and dbms need to
      have very tight control of security data (guarantee that pages will be
      load or persisted)

    Query processor

    - Query is parsed and simplified (parser and ReWriter)
     By parsed means translating to tree representation and after that system
     simplyfies that query.
     Then query optimizer tries to rewrite query too by trying to predict
     execution cost of diffirent queries and then it selects the cheapest one.
     Then query is executed.
    - Query plans are tree where leafs corresponds to datat sources (tables)

    Query Executer:

    - Each standard operator (projection, join...) has multiple implementations
      which executer selects.
    - Query optimizer calculate estimation cost for different query
      implementations and index usage and selects the fastests one;
      BUT sometimes optimizer selects wrong plan so there is a need to do it by
      our selves.

    SUMMARY ABOUT COST CALCULATIONS:
    Scan - expensive because it need chech every page;
    BTree - much better because it scans nodes of indexes and then read entries;
    BTree index with datat -> the fastest because there no need to read
    anything;
    Unclustered index -> could be more expensive because it need to check nodes
    and then read records from hard disk.

    An example
    -if we do select like SELECT count(*) from sometable where somecolumn =/</>
    'something' (somecolumn has Unclustered index) then optimizer will use
    index scan because we don't need data at the end, just count where system
    can count just index;
    -if we do select like SELECT someDifferentColumn from sometable where somecolumn =/</>
    'something' (somecolumn has Unclustered index) then optimizer will not use
    index scan, because it will need to scan index and then retreive data from
    table, better to do simple scan. But the selection depends on data that will
    be retrieved. Probably in some cases is cheaper to use index when we going
    to get a small amount of rows.

    TIPS:
    Idex or sort orders can speed up filtering;
    Sometimes we can improve query with joins by swapping them i.e. if outer
    table contains more rows rather than inener table then it's better to swap
    them.
    HASH join cost cheaper rather than using indexes.

