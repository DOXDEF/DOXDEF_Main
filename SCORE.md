### Individual Score

| Midterm | Final | Total |
| ------- | ----- | ----- |
| 113 | 86 | 87.760 |
| 104 | 83 | 83.080 |
| 110 | 78 | 82.000 |
| 101 | 79 | 79.720 |
| 101 | 77 | 78.520 |
| 103 | 76 | 78.560 |
| 71 | 94 | 79.120 |
| 102 | 71 | 75.240 |
| 88 | 76 | 73.760 |
| 101 | 72 | 75.520 |
| 81 | 78 | 72.720 |
| 84 | 78 | 73.680 |
| 91 | 72 | 72.320 |
| 107 | 65 | 73.240 |
| 64 | 89 | 73.880 |
| 78 | 86 | 76.560 |
| 74 | 73 | 67.480 |
| 117 | 45 | 64.440 |
| 100 | 55 | 65.000 |
| 105 | 53 | 65.400 |
| 72 | 88 | 75.840 |
| 87 | 73 | 71.640 |
| 44 | 73 | 57.880 |
| 66 | 81 | 69.720 |
| 64 | 70 | 62.480 |
| 86 | 51 | 58.120 |
| 68 | 52 | 52.960 |
| 47 | 66 | 54.640 |
| 63 | 66 | 59.760 |
| 51 | 57 | 50.520 |
| 64 | 41 | 45.080 |
| 54 | 49 | 46.680 |
| 53 | 47 | 45.160 |
| 92 | 29 | 46.840 |
| 48 | 18 | 26.160 |
| 42 | 36 | 35.040 |
| 21 | 22 | 19.920 |
| 55 | 55 | 50.600 |
| 61 | 28 | 36.320 |
| 50 | 35 | 37.000 |
| 21 | 0 | 6.720 |
| 45 | 0 | 14.400 |
| 42 | 0 | 13.440 |

### Examination Analysis
* Midterm
  * Mean: **74.209**
  * Variance: 632.817
  * Median: **72.000**
  * Min/Max: (21.000, 117.000)
* Final
  * Mean: **58.674**
  * Variance: 618.545
  * Median: **66.000**
  * Min/Max: (0.000, 94.000)
* Total
  * Mean: **58.952**
  * Variance: 423.546
  * Median: **65.000**
  * Min/Max: (6.720, 87.760)

-------------Python Code-------------
=====================================

```python
import numpy

def read_data(filename):
    data = []
    with open(filename, 'r') as f:
        for line in f:
            if line.startswith('#'):
                continue
            values = []
            for word in line.split(','):
                values.append(int(word))
            data.append(values)
        return data

def add_weighted_average(data, weight):
    for row in data:
        average = weight[0] * row[0] + weight[1] * row[1]
        row.append(average)
        
def get_median(data):
    data = sorted(data) 
    centerIndex = len(data)//2
    return (data[centerIndex ] + data[-centerIndex - 1])/2

def analyze_data(data):
    mean = sum(data) / len(data)
    var = numpy.var(data)
    median = get_median(data)
    return mean, var, median, min(data), max(data)

if __name__ == '__main__':
    data = read_data('data/class_score_en.csv')
    if data and len(data[0]) == 2:
        add_weighted_average(data, [40/125, 60/100])
        if len(data[0]) == 3:
            print('### Individual Score')
            print()
            print('| Midterm | Final | Total |')
            print('| ------- | ----- | ----- |')
            for row in data:
                print(f'| {row[0]} | {row[1]} | {row[2]:.3f} |')
            print()

            print('### Examination Analysis')
            col_n = len(data[0])
            col_name = ['Midterm', 'Final', 'Total']
            colwise_data = [ [row[c] for row in data] for c in range(col_n) ]
            for c, score in enumerate(colwise_data):
                mean, var, median, min_, max_ = analyze_data(score)
                print(f'* {col_name[c]}')
                print(f'  * Mean: **{mean:.3f}**')
                print(f'  * Variance: {var:.3f}')
                print(f'  * Median: **{median:.3f}**')
                print(f'  * Min/Max: ({min_:.3f}, {max_:.3f})')
```
