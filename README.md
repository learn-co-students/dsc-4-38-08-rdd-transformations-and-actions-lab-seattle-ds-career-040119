
# RDD Transformations and Actions - Lab

## Introduction
In previous lab, we saw how to create an RDD from a Python list object, set the number of logical partitions and change basic RDD properties. We also looked at how RDD operations can be split into actions and transformations. In this lesson, we will practice some basic actions and transformations on the RDD we created in the previous lab. 

Note: Your are advised to visit all the included external links to see more examples of stated concepts.

## Objectives

You will be able to:

* Apply the `map(func)` transformation to a given function on all elements of an RDD in different partitions
* Use `collect()` action to trigger the processing stage of spark's lazy evaluation
* Use `count()` action to calculate the number of elements of a parallelized RDD
* Use `filter(func)` to filter unwanted data from RDDs
* Develop an understanding of Python's lambda functions for RDDs processing

## Start with a parallelized RDD

Let's first import the intRDD we created earlier and tranform the elements using `map()`.

- Import the code from previous lab


```python
# Include the code from previous lab to create an intRDD[1,2,..,1000]

# Code here 

```

At this point, we have created an RDD containing a sequence of numbers and split it into a number of partitions. We shall now try to execute basic data manipulation operations on this RDD and inspect the outcome. Data analysis usually requires the analyst to perform certain operations on every element of a dataset. In Spark such analyses tasks are run in parallel to process a subset of data in parallel to other subsets. 

## The `map(func)` "Transformation" Operation 

`map(func)` is the most commonly used and one of the basic transformations in Spark. It applies a function `func` to each data element of an RDD and outputs a resulting dataset. Running `map(func)` on a datase launches a **single stage** of tasks. A stage is a group of tasks that all perform the same computation, but on different subsets of data. Tasks are launched for each partition as shown below:

![](tasks.png)

a **TASK** is a unit of execution that runs on a single machine. When we execute `map(func)` in a partition, a new task applies `func` to all entries of data in that partition, outputting a new partition. In the example above, a dataset has been broken into 4 partitions, so four `map(func)` tasks are launched in each partition. 

Following figure shows this mechanism for a smaller example, similar to our `intRDD` . 

![](map.png)
#### `map(func)` : Each task creates a new partition by calling `func(e)` on each element `e` from the original partition. 



### Applying Map transformation

When applying a `map(func)` transformation, each item in the parent RDD maps to one element in the resuting RDD i.e. the parent RDD `intRDD` has 100 elements, the new RDD post map transformation will also have 100 elements. 

Let's try to subtract 1 from each value in the intRDD.

1. create a function `subtract()` to subtract 1 from input integer. 
2. pass each element from `intRDD` to `map(func)` where func refers to the `subtract()` function from step 1. 
3. Use `toDebugString()` to see the transformation lineage. 


```python
# Create sub function to subtract 1

def subtract(value):
    
# Code here 

    pass

# Transform intRDD through map transformation using sub function
# Because map is a transformation and Spark uses lazy evaluation, no jobs, stages,
# or tasks will be launched when we run this code.


# Code here 


# Let's see the RDD transformation hierarchy


# Code here 




# b'(5) PythonRDD[1] at RDD at PythonRDD.scala:49 []\n 
# |  ParallelCollectionRDD[0] at parallelize at PythonRDD.scala:184 []'
```

    b'(4) PythonRDD[1] at RDD at PythonRDD.scala:53 []\n |  ParallelCollectionRDD[0] at parallelize at PythonRDD.scala:195 []'


## The `collect()` "action" method

If we want to view the contents of resulting RDD i.e. `subtractRDD`, we would need to create a new list on the driver from the data distributed in partitions. The `RDD.collect()` method is used for this purpose. You must be careful when using the collect method to ensure that the driver has enough memory for the collected data, or the driver may crash. 

- Consult the official documentation and use `RDD.collect()` to view the contents of `intRDD`. 


```python
# Let's collect the data using .collect on SubtractRDD


# Code here 

```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99, 100, 101, 102, 103, 104, 105, 106, 107, 108, 109, 110, 111, 112, 113, 114, 115, 116, 117, 118, 119, 120, 121, 122, 123, 124, 125, 126, 127, 128, 129, 130, 131, 132, 133, 134, 135, 136, 137, 138, 139, 140, 141, 142, 143, 144, 145, 146, 147, 148, 149, 150, 151, 152, 153, 154, 155, 156, 157, 158, 159, 160, 161, 162, 163, 164, 165, 166, 167, 168, 169, 170, 171, 172, 173, 174, 175, 176, 177, 178, 179, 180, 181, 182, 183, 184, 185, 186, 187, 188, 189, 190, 191, 192, 193, 194, 195, 196, 197, 198, 199, 200, 201, 202, 203, 204, 205, 206, 207, 208, 209, 210, 211, 212, 213, 214, 215, 216, 217, 218, 219, 220, 221, 222, 223, 224, 225, 226, 227, 228, 229, 230, 231, 232, 233, 234, 235, 236, 237, 238, 239, 240, 241, 242, 243, 244, 245, 246, 247, 248, 249, 250, 251, 252, 253, 254, 255, 256, 257, 258, 259, 260, 261, 262, 263, 264, 265, 266, 267, 268, 269, 270, 271, 272, 273, 274, 275, 276, 277, 278, 279, 280, 281, 282, 283, 284, 285, 286, 287, 288, 289, 290, 291, 292, 293, 294, 295, 296, 297, 298, 299, 300, 301, 302, 303, 304, 305, 306, 307, 308, 309, 310, 311, 312, 313, 314, 315, 316, 317, 318, 319, 320, 321, 322, 323, 324, 325, 326, 327, 328, 329, 330, 331, 332, 333, 334, 335, 336, 337, 338, 339, 340, 341, 342, 343, 344, 345, 346, 347, 348, 349, 350, 351, 352, 353, 354, 355, 356, 357, 358, 359, 360, 361, 362, 363, 364, 365, 366, 367, 368, 369, 370, 371, 372, 373, 374, 375, 376, 377, 378, 379, 380, 381, 382, 383, 384, 385, 386, 387, 388, 389, 390, 391, 392, 393, 394, 395, 396, 397, 398, 399, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 418, 419, 420, 421, 422, 423, 424, 425, 426, 427, 428, 429, 430, 431, 432, 433, 434, 435, 436, 437, 438, 439, 440, 441, 442, 443, 444, 445, 446, 447, 448, 449, 450, 451, 452, 453, 454, 455, 456, 457, 458, 459, 460, 461, 462, 463, 464, 465, 466, 467, 468, 469, 470, 471, 472, 473, 474, 475, 476, 477, 478, 479, 480, 481, 482, 483, 484, 485, 486, 487, 488, 489, 490, 491, 492, 493, 494, 495, 496, 497, 498, 499, 500, 501, 502, 503, 504, 505, 506, 507, 508, 509, 510, 511, 512, 513, 514, 515, 516, 517, 518, 519, 520, 521, 522, 523, 524, 525, 526, 527, 528, 529, 530, 531, 532, 533, 534, 535, 536, 537, 538, 539, 540, 541, 542, 543, 544, 545, 546, 547, 548, 549, 550, 551, 552, 553, 554, 555, 556, 557, 558, 559, 560, 561, 562, 563, 564, 565, 566, 567, 568, 569, 570, 571, 572, 573, 574, 575, 576, 577, 578, 579, 580, 581, 582, 583, 584, 585, 586, 587, 588, 589, 590, 591, 592, 593, 594, 595, 596, 597, 598, 599, 600, 601, 602, 603, 604, 605, 606, 607, 608, 609, 610, 611, 612, 613, 614, 615, 616, 617, 618, 619, 620, 621, 622, 623, 624, 625, 626, 627, 628, 629, 630, 631, 632, 633, 634, 635, 636, 637, 638, 639, 640, 641, 642, 643, 644, 645, 646, 647, 648, 649, 650, 651, 652, 653, 654, 655, 656, 657, 658, 659, 660, 661, 662, 663, 664, 665, 666, 667, 668, 669, 670, 671, 672, 673, 674, 675, 676, 677, 678, 679, 680, 681, 682, 683, 684, 685, 686, 687, 688, 689, 690, 691, 692, 693, 694, 695, 696, 697, 698, 699, 700, 701, 702, 703, 704, 705, 706, 707, 708, 709, 710, 711, 712, 713, 714, 715, 716, 717, 718, 719, 720, 721, 722, 723, 724, 725, 726, 727, 728, 729, 730, 731, 732, 733, 734, 735, 736, 737, 738, 739, 740, 741, 742, 743, 744, 745, 746, 747, 748, 749, 750, 751, 752, 753, 754, 755, 756, 757, 758, 759, 760, 761, 762, 763, 764, 765, 766, 767, 768, 769, 770, 771, 772, 773, 774, 775, 776, 777, 778, 779, 780, 781, 782, 783, 784, 785, 786, 787, 788, 789, 790, 791, 792, 793, 794, 795, 796, 797, 798, 799, 800, 801, 802, 803, 804, 805, 806, 807, 808, 809, 810, 811, 812, 813, 814, 815, 816, 817, 818, 819, 820, 821, 822, 823, 824, 825, 826, 827, 828, 829, 830, 831, 832, 833, 834, 835, 836, 837, 838, 839, 840, 841, 842, 843, 844, 845, 846, 847, 848, 849, 850, 851, 852, 853, 854, 855, 856, 857, 858, 859, 860, 861, 862, 863, 864, 865, 866, 867, 868, 869, 870, 871, 872, 873, 874, 875, 876, 877, 878, 879, 880, 881, 882, 883, 884, 885, 886, 887, 888, 889, 890, 891, 892, 893, 894, 895, 896, 897, 898, 899, 900, 901, 902, 903, 904, 905, 906, 907, 908, 909, 910, 911, 912, 913, 914, 915, 916, 917, 918, 919, 920, 921, 922, 923, 924, 925, 926, 927, 928, 929, 930, 931, 932, 933, 934, 935, 936, 937, 938, 939, 940, 941, 942, 943, 944, 945, 946, 947, 948, 949, 950, 951, 952, 953, 954, 955, 956, 957, 958, 959, 960, 961, 962, 963, 964, 965, 966, 967, 968, 969, 970, 971, 972, 973, 974, 975, 976, 977, 978, 979, 980, 981, 982, 983, 984, 985, 986, 987, 988, 989, 990, 991, 992, 993, 994, 995, 996, 997, 998, 999]


We can see that 1 has been subtracted from our original list 1 - 1000 , which now contains a 0 - 999. 

>The `collect()` method is the first **action** operation that we have encountered. Action operations cause Spark to perform the (lazy) transformation operations that are required to compute the RDD returned by the action. In our example, this means that tasks will now be launched to perform the parallelize, map, and collect operations.

Have a look at following figure to further strengthen the intuition around the `collect()` method. 

![](collect.png)




## The `count()` "action" method

`count()` is another basic action that is used to count the number of elements in an RDD. Since `map()` creates new RDD with same number of elements as base RDD, applying `count()` to base and resulting RDD should return the same result. Remember `count()` is an action operation. Had we not called `collect()` earlier, Spark would now perform the evaluation. 

- Use `RDD.count()` to count elements in `intRDD` and `subtractRDD` to see if it meets our expectations.   


```python
# Count the elements in base and resulting RDDs

# Code here 

# 1000 1000
```

    1000 1000


Each task counts the entries in its partition and sends the result to  SparkContext, which adds up all of the counts. The figure below shows what would happen if we ran count() on a small example dataset with just four partitions.
![](count.png)


## The `filter(func)` Transformation

We shall now create another RDD which would only contain values less than threshold, say 10. A `filter(func)`operation is used for filtering out unwanted data elements. `filter(func)` is a transformation method that creates a new RDD by applying `func` to each element of the parent RDD and only returns those values where `func` returns a `True` value, dropping other elements.

Let's try to apply the filter transformation using steps similar to `map(func)` seen earlier. 

1. Create a function `lessThanTen()` that takes an integer values to identify if it is < 10
2. Pass the `subRDD` elements as input to `lessThanTen()` to check for the set condition in step 1.
3. return a True or a False value back to `filter()` to be stored in `filteredRDD`.
4. Collect to trigger the transformation and view the results of `filteredRDD`.



```python
# Define a function to filter a single value
def lessThanTen(value):

    pass

# Pass the function ten to the filter transformation
# Filter is a transformation so no tasks are run


# Code here 


# View the results using collect()
# Collect is an action and triggers the filter transformation to run

# Code here 




# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



Following image shows a distinction between the map and filter function mentioned above
![](mf.png)

## Lambda Functions

Python supports the use of small one-line anonymous functions that are not bound to a name at runtime. Borrowed from LISP, these lambda functions can be used wherever function objects are required. They are syntactically restricted to a single expression. Remember that lambda functions are a matter of style and using them is never mandatory. You can always define a separate normal function instead, but using a lambda() function is an equivalent and more compact form of coding.

- Lets try to implement the filter as shown above using lambda function in a single line. 


```python
# Apply the above function with filter(func) transformation using Python's lambda functions


# Code here 


# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```




    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]



Similarly , we can apply other filters. 

- Implement a filter than only outputs even values higher than 10 and less than 30. 


```python
# Let's collect the even values lgreater than 10 and less than 30

# Code here 


# [12, 14, 16, 18, 20, 22, 24, 26, 28]
```




    [12, 14, 16, 18, 20, 22, 24, 26, 28]



## Further Reading 

- [Programming with RDDs](https://runawayhorse001.github.io/LearningApacheSpark/rdd.html)

## Summary

In this lab, we saw how we can apply basic transformations and actions on the data stofred within RDDs. We also looked at the Lazy Evaluation performed by Spark and learnt to differentiate between transformations and actions. We saw how lambda functions can be used as a one-line approach towards declaring functions in Spark. In next lessons, we shall see more transformations and actions on RDDs. 
