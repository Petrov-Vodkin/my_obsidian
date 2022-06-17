2022-05-24 09:20
#tag
# Алгоритмы py
## [Бинарный поиск](https://pythonpip.ru/examples/dvoichnyy-poisk-python)
-это алгоритм поиска определенного элемента в упорядоченном списке списке
-предполагает итеративное деление списка пополам, где на каждом шаге искомый элемент сравнивается со значением среднего элемента в отсортированном списк
```python
def binary_search_recursive(array: list, element: (int, float), start, end):  
    if start > end:  
        return -1  
  
    mid = (start + end) // 2  
    if element == array[mid]:  
        return mid  
  
    if element < array[mid]:  
        return binary_search_recursive(array, element, start, mid-1)  
    else:  
        return binary_search_recursive(array, element, mid+1, end)  
  
element = 18  
array = [1, 2, 5, 7, 13, 15, 16, 18, 24, 28, 29]  
  
print(f"Searching for {element}")  
print(f"Index of {element}: {binary_search_recursive(array, element, 0, len(array))}")
```
_____________
## Links
[[Python]]