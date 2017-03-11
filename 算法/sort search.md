```排序```

	func quickSort(data []int, low int, hight int) {
	//low=0
	//hight=len(data)-1

	if low < hight {
		i := low
		j := hight
		temp := data[low]
		for i < j {
			for i < j && data[j] >= temp {
				j--
			}
			data[i] = data[j]
			for i < j && data[i] <= temp {
				i++
			}
			data[j] = data[i]
		}
		data[i] = temp
		quickSort(data, low, i-1)
		quickSort(data, i+1, hight)
	}
	}



```查找```

	func binSearch(list []int, low int, high int, item int) int {

	for low <= high {
		mid := low + (high-low)/2
		if item < list[mid] {
			high = mid - 1
		} else if item > list[mid] {
			low = mid + 1
		} else {
			return mid
		}
	}
	return -1
	}

