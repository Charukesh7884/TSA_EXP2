# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
### Date:17/10/2026
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


data = pd.read_csv('/content/NVIDIA_STOCK.csv', header=0)

data['Price'] = pd.to_datetime(data['Price'], errors='coerce')
data = data.dropna(subset=['Price'])
data.set_index('Price', inplace=True)
data['Close'] = pd.to_numeric(data['Close'], errors='coerce')

resampled_data = data['Close'].resample('YE').mean().to_frame()

resampled_data.head()

resampled_data.index = resampled_data.index.year
resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Month': 'Year'}, inplace=True)
resampled_data.head()

years = resampled_data['Price'].tolist()

passengers = resampled_data['Close'].tolist()
X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, passengers)]

coeff = [[len(X), sum(X), sum(x2)],
         [sum(X), sum(x2), sum(x3)],
         [sum(x2), sum(xy), sum(x4)]]

n = len(years)
b = (n * sum(xy) - sum(passengers) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
a = (sum(passengers) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]
x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, passengers)]

coeff = [[len(X), sum(X), sum(x2)],
         [sum(X), sum(x2), sum(x3)],
         [sum(x2), sum(x3), sum(x4)]]

Y = [sum(passengers), sum(xy), sum(x2y)]
A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution
poly_trend = [a_poly + b_poly * X[i] + c_poly * (X[i] ** 2) for i in range(n)]

print(f"Linear Trend: y={a:.2f} + {b:.2f}x")
print(f"\nPolynomial Trend: y={a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

resampled_data.set_index('Price',inplace=True)
```
A - LINEAR TREND ESTIMATION
```python
resampled_data['Close'].plot(kind='line', color='black', marker='o', label='Actual Stock Price')
resampled_data['Linear Trend'].plot(kind='line', color='red', linestyle='--', label='Linear Trend')
```
B- POLYNOMIAL TREND ESTIMATION
```python
resampled_data['Close'].plot(kind='line', color='blue', marker='o', label='Actual Stock Price')
resampled_data['Polynomial Trend'].plot(kind='line', color='black', marker='o', label='Polynomial Trend')
```
### OUTPUT

A - LINEAR TREND ESTIMATION

<img width="775" height="585" alt="image" src="https://github.com/user-attachments/assets/61a9055f-dd9b-4f2f-9d95-bdcfd89a19b9" />



B- POLYNOMIAL TREND ESTIMATION
<img width="761" height="596" alt="image" src="https://github.com/user-attachments/assets/70c1e823-b160-4e02-942a-9b4811a3f58f" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
