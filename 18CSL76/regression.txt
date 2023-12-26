import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

def kernel(point, xmat, k):
    m, n = np.shape(xmat)
    weights = np.mat(np.eye((m)))
    for j in range(m):
        diff = point - xmat[j]
        weights[j, j] = np.exp(diff * diff.T / (-2.0 * k**2))
    return weights

def localWeight(point, xmat, ymat, k):
    wei = kernel(point, xmat, k)
    B = (xmat.T * (wei * xmat)).I * (xmat.T * (wei * ymat.T))
    return B

def localWeightRegression(xmat, ymat, k):
    m, n = np.shape(xmat)
    ypred = np.zeros(m)
    for i in range(m):
        ypred[i] = xmat[i] * localWeight(xmat[i], xmat, ymat, k)
    return ypred

# Load data points
data = pd.read_csv('prog9.csv')
bill = np.array(data.total_bill)
tip = np.array(data.tip)

# Preparing and add 1 in bill
mbill = np.mat(bill)
mtip = np.mat(tip)
m = np.shape(mbill)[1]
one = np.mat(np.ones(m))
X = np.hstack((one.T, mbill.T))  # Set k here
ypred = localWeightRegression(X, mtip, 0.3)

# Add predicted values to the DataFrame
data['Predicted_Tip'] = ypred

# Save the DataFrame to a new CSV file
data.to_csv('predicted_tips.csv', index=False)
