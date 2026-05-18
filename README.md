# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
## Date:16/5/26
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
1.Import necessary libraries (NumPy, Matplotlib)
2.Load the dataset
3.Calculate the linear trend values using least square method
4.Calculate the polynomial trend values using least square method
5.End the program
### PROGRAM:
### Name: Navinkumar V
### Reg No: 212223230141
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
df = pd.read_csv("gold_rate_history.csv")
df.head()

df['Date'] = pd.to_datetime(df['Date'], format='mixed')
df = df.set_index('Date')
df.head()

resampled_data = df['rate'].resample('Y').sum().to_frame()
resampled_data.head()
resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.head()
date = resampled_data['Date'].tolist()
resampled_data['Date'] = date
price = resampled_data['rate'].tolist()
```
### A - LINEAR TREND ESTIMATION
```
X = [i - date[len(date) // 2] for i in date]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, price)]
n = len(date)
b = (n * sum(xy) - sum(price) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
a = (sum(price) - b * sum(X)) / n
linear_trend = [a + b * X[i] for i in range(n)]
```
### B- POLYNOMIAL TREND ESTIMATION
```
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, price)]
coeff = [[len(X), sum(X), sum(x2)],
[sum(X), sum(x2), sum(x3)],
[sum(x2), sum(x3), sum(x4)]]
Y = [sum(price), sum(xy), sum(x2y)]
A = np.array(coeff)
B = np.array(Y)
solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution
poly_trend = [a_poly + b_poly * X[i] + c_poly * (X[i] ** 2) for i in range(n)]
```
### plot
```
print(f"Linear Trend: y={a:.2f} + {b:.2f}x")
print(f"\nPolynomial Trend: y={a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend
resampled_data['rate'].plot(kind='line',color='blue',marker='o') #alpha=0.3 makes 
resampled_data['Linear Trend'].plot(kind='line',color='black',linestyle='--')
resampled_data['rate'].plot(kind='line',color='blue',marker='o')
resampled_data['Polynomial Trend'].plot(kind='line',color='black',marker='o')

```
### OUTPUT
### A - LINEAR TREND ESTIMATION
<img width="304" height="26" alt="image" src="https://github.com/user-attachments/assets/7aa8c5ce-515f-44ea-84f8-89db71965fed" />
<img width="542" height="437" alt="image" src="https://github.com/user-attachments/assets/f85c611f-c3f6-4bc4-870d-f2da0ba3b551" />

### B- POLYNOMIAL TREND ESTIMATION
<img width="430" height="34" alt="image" src="https://github.com/user-attachments/assets/e18787b9-96e6-4e8b-9d7c-ba741f2a9c77" />
<img width="532" height="430" alt="image" src="https://github.com/user-attachments/assets/14909119-675b-41cc-a46e-36205599c99c" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
