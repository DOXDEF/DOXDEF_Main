### Korean Population by Region

* Total population: 51669716

| Region | Population | Ratio (%) |
| ------ | ---------- | --------- |
| Seoul | 9550227 | 18.5 |
| Gyeongi | 13530519 | 26.2 |
| Busan | 3359527 | 6.5 |
| Gyeongnam | 3322373 | 6.4 |
| Incheon | 2938429 | 5.7 |
| Gyeongbuk | 2630254 | 5.1 |
| Daegu | 2393626 | 4.6 |
| Chungnam | 2118183 | 4.1 |
| Jeonnam | 1838353 | 3.6 |
| Jeonbuk | 1792476 | 3.5 |
| Chungbuk | 1597179 | 3.1 |
| Gangwon | 1536270 | 3.0 |
| Daejeon | 1454679 | 2.8 |
| Gwangju | 1441970 | 2.8 |
| Ulsan | 1124459 | 2.2 |
| Jeju | 675883 | 1.3 |
| Sejong | 365309 | 0.7 |

### Korean Covid-19 New Cases by Region

* Total new cases: 1714

| Region | New Cases | Ratio (%) | New Cases / 1M |
| ------ | --------- | --------- | -------------- |
| Seoul | 644 | 37.6 | 67.4 |
| Gyeongi | 529 | 30.9 | 39.1 |
| Busan | 38 | 2.2 | 11.3 |
| Gyeongnam | 29 | 1.7 | 8.7 |
| Incheon | 148 | 8.6 | 50.4 |
| Gyeongbuk | 28 | 1.6 | 10.6 |
| Daegu | 41 | 2.4 | 17.1 |
| Chungnam | 62 | 3.6 | 29.3 |
| Jeonnam | 23 | 1.3 | 12.5 |
| Jeonbuk | 27 | 1.6 | 15.1 |
| Chungbuk | 27 | 1.6 | 16.9 |
| Gangwon | 33 | 1.9 | 21.5 |
| Daejeon | 16 | 0.9 | 11.0 |
| Gwangju | 40 | 2.3 | 27.7 |
| Ulsan | 20 | 1.2 | 17.8 |
| Jeju | 5 | 0.3 | 7.4 |
| Sejong | 4 | 0.2 | 10.9 |


-------------Python Code-------------
=====================================

```python
def normalize_data(n_cases, n_people, scale):
    norm_cases = []
    for idx, n in enumerate(n_cases):
        norm_cases.append(n_cases[idx] / n_people[idx] * scale)
    return norm_cases

regions  = ['Seoul', 'Gyeongi', 'Busan', 'Gyeongnam', 'Incheon', 'Gyeongbuk', 'Daegu', 'Chungnam', 'Jeonnam', 'Jeonbuk', 'Chungbuk', 'Gangwon', 'Daejeon', 'Gwangju', 'Ulsan', 'Jeju', 'Sejong']
n_people = [9550227,  13530519, 3359527,     3322373,   2938429,     2630254, 2393626,    2118183,   1838353,   1792476,    1597179,   1536270,   1454679,   1441970, 1124459, 675883,   365309] # 2021-08
n_covid  = [    644,       529,      38,          29,       148,          28,      41,         62,        23,        27,         27,        33,        16,        40,      20,      5,        4] # 2021-09-21

sum_people = sum(n_people) 
sum_covid  = sum(n_covid) 
norm_covid = normalize_data(n_covid, n_people, 1000000) 


print('### Korean Population by Region')
print('* Total population:', sum_people)
print('| Region | Population | Ratio (%) |')
print('| ------ | ---------- | --------- |')
for idx, pop in enumerate(n_people):
    ratio = pop / sum_people * 100 
    print('| %s | %d | %.1f |' % (regions[idx], pop, ratio))
print('')



print('### Korean Covid-19 New Cases by Region')
print('* Total new cases:', sum_covid)
print('| Region | New Cases | Ratio (%) | New Cases / 1M |')
print('| ------ | --------- | --------- | -------------- |')
for idx, pop in enumerate(n_covid):
    ratio = pop / sum_covid * 100 
    print('| %s | %d | %.1f | %.1f |' % (regions[idx], pop, ratio, norm_covid[idx]))
```
