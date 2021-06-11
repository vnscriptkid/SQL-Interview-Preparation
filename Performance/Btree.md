https://www.youtube.com/watch?v=aZjYr87r1b8

## 1. Disk structure? How data is stored in disk?

- Any piece of data in hard disk can be accessed by: Block address + offset
  - Block address = (track No + sector No)
  - Block size = 512 bytes 
- We can't read data directly from disk, must put it into RAM first
- DBMS vs Data structure:
  - DBMS: how data is organized inside disk
  - DS: how data is organized inside RAM  

## 2. What is indexing?
#### :one: Without indexing
- Assuming our table holds 100 records, each record is 128 bytes.
  - 512 / 128 = 4 : One block can store 4 records
  - 100 records need 25 blocks
- Normal search without indexing requires pulling all 100 records from disk to RAM ~ 12800 bytes ~ 12 MB before searching every record one by one.

#### :two: With indexing: 
- Allows faster data access. But how? Index is actually a table inside database, storing at least 2 information
  - value of indexed column 
  - place where actual row resides in disk

:point_right: We need much less disk capacity storing index table, comparing to capacity for whole table

⚖️ Let's say, it takes 16 bytes for each record of index table (vs 128 bytes of original row)

- :point_right: 512 / 16 = 32 :point_right: 1 block can store 32 records 
- :point_right: 100 records need ~ 3.2 blocks ( vs 25 blocks of original table) 

- When acessing data with indexing support:
  - We pull 100 records of index table from disk to RAM ~ 16 bytes * 100 ~ 1600 bytes ~ 1.5 MB
  - We then do search on the data structure that indexing use (B-tree maybe)
  - Eventually, we find the block where data we need is stored, pull it from disk to RAM (+1 block size for RAM) 

:question: :confused: :question: Why it's fast? 
  -  Stuffing less data to RAM from disk (1.5MB vs 12MB)
  -  We're not doing sequencial search but search on a tree, which takes :rocket: __O(logn)__ :rocket: time

## 4. What is multi-indexing? (continued)
 
## 5. M-way search trees?
- For binary search tree
  - 1 node has 1 key and 2 children
  - 1 key is used to determine which way to go, either left or right

- For m-way search tree 
  - 1 node has __(m - 1) keys__ and __m chidlren__.
  - __(m - 1) keys__ are used to determine which way to go, we need __(m - 1) keys__ to represent m choices

- Example: for 4-way search tree
  - 1 node has __4 children__
  - there's __3 keys__. Let's say: 20 , 70 , 100 . These 3 keys our whole space into: (--20--70--100--)
    - (1) < 20
    - (2) >=20 && <70
    - (3) >70 && < 100
    - (4) <= 100  

- :worried: Problems with m-way search tree:
  - :palm_tree: No way to control the height of tree
  - :thumbsdown: In worst case, the height of the tree can be up to the number of nodes

## 6. B tree?
- What? improved version of m-way search tree with height control
- All constraints:
+ Root can have minimum of 2 children
+ Other nodes must have at least m/2 children
+ All leaf at the same level
+ Bottom-up

## 7. Insertion and Deletion B tree? (continued)

## 8. B+  tree? 
- For B tree, each node holds a pointer to actual record
- For B+ tree, only leaf nodes hold pointers to actual record

https://www.youtube.com/watch?v=C_q5ccN84C8

![image](https://user-images.githubusercontent.com/28957748/121667647-b5fbfd00-cad4-11eb-981e-4d3d3fb512b6.png)
