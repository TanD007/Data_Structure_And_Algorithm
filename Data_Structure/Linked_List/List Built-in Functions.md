![][image1]

**Introduction to Basic Data Structures**

**List Built-in Functions:** 

1. **Constructor**

| Name | Details | Time Complexity |
| :---- | :---- | :---- |
| **list\<type\>myList;** | Construct a list with 0 elements. | O(1) |
| **list\<type\>myList(N);** | Construct a list with N elements and the value will be garbage. | O(N) |
| **list\<type\>myList(N,V);** | Construct a list with N elements and the value will be V. | O(N) |
| **list\<type\>myList(list2);** | Construct a list by copying another list list2. | O(N) |
| **list\<type\>myList(A,A+N);** | Construct a list by copying all elements from an array A of size N. | O(N) |
| **list\<type\>myList(v.begin(),v.end());** | Construct a list by copying all elements from a vector v. | O(N) |

2. **Capacity**

	

| Name | Details | Time Complexity |
| :---- | :---- | :---- |
| **myList.size()** | Returns the size of the list. | O(1) |
| **myList.max\_size()** | Returns the maximum size that the list can hold. | O(1) |
| **myList.clear()** | Clears the list elements.  | O(N) |
| **myList.empty()** | Return true/false if the list is empty or not. | O(1)  |
| **myList.resize()** | Change the size of the list. | O(K); where K is the difference between new size and current size. |

3. **Modifiers**

| Name | Details | Time Complexity |
| :---- | :---- | :---- |
| **myList= or myList.assign(list2.begin(),list2.end())** | Assign another list. | O(N) |
| **myList.push\_back()** | Add an element to the tail. | O(1) |
| **myList.push\_front()** | Add an element to the head. | O(1) |
| **myList.pop\_back()** | Delete the tail. | O(1) |
| **myList.pop\_front()** | Delete the head. | O(1) |
| **myList.insert()** | Insert elements at a specific position. | O(N+K); where K is the number of elements to be inserted. |
| **myList.erase()** | Delete elements from a specific position. | O(N+K); where K is the number of elements to be deleted.  |
| **replace(myList.begin(),myList.end(),value,replace\_value)** | Replace all the value with replace\_value. Not under a list STL. | O(N) |
| **find(myList.begin(),myList.end(),V)** | Find the value V. Not under a list STL. | O(N) |

   

   

   

   

   

   

   

   

   

   

4. **Operations**

| Name | Details | Time Complexity |
| :---- | :---- | :---- |
| **myList.remove(V)** | Remove the value V from the list. | O(N) |
| **myList.sort()** | Sort the list in ascending order. | O(NlogN) |
| **myList.sort(greater\<type\>())** | Sort the list in descending order | O(NlogN) |
| **myList.unique()** | Deletes the duplicate values from the list. You must sort the list first. | O(N), with sort O(NlogN) |
| **myList.reverse()** | Reverse the list. | O(N) |

5. **Element access**

| Name | Details | Time Complexity |
| :---- | :---- | :---- |
| **myList.back()** | Access the tail element. | O(1) |
| **myList.front()** | Access the head element. | O(1) |
| **next(myList.begin(),i)** | Access the ith element | O(N) |

6. **Iterators**

| Name | Details | Time Complexity |
| :---- | :---- | :---- |
| **myList.begin()** | Pointer to the first element. | O(1) |
| **myList.end()** | Pointer to the last element. | O(1) |

[image1]: <data:image/png;base64,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>