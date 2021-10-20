STL的sort()算法，数据量大时采用Quick Sort，分段递归排序。一旦分段后的数据量小于某个阈值，为避免Quick Sort的递归调用带来过大的额外开销，就改用Insertion Sort（插入排序）。如果递归层次过深，还会改用Heap Sort。

如果我们拿Insertion Sort来处理大数据量，其O(N^2)的复杂度无法令人接受。大数据量有更好的排序算法可供选择。快排是目前已知的最快的排序方法，平均复杂度为O(N log N),最坏情况O(N^2)。不过IntroSort（极其类似 median-of - three QuickSort的一种排序算法）可将最坏情况推进到O（NlogN），早期的STL sort算法都采用QuickSort，SGI STL已经改用IntroSort。

QuickSort的精神在于将大区间分割为小区间，分段排序，每一个小区间排序完成后串接起来大区间也就完成了排序。最坏的情况发生在分割时产生了一个空的子区间 - 那完全没有达到分割的预期效果。

面对只有十来个元素的小型序列，用像QuickSort这样复杂而可能需要大量运算的快速排序似乎显得不划算。所以，在小数据量的情况下，InsertionSort也可能快过QuickSort，因为Quick会为了极小的子序列而产生许多的函数递归调用。

基于这种情况，适度评估序列的大小，然后决定采用InsertionSort还是QuickSort。