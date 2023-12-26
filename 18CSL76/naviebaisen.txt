import pandas as pd
from sklearn.preprocessing import LabelEncoder
from sklearn.naive_bayes import GaussianNB
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score

data = pd.read_csv('prog4.csv')
print("The first 5 Values of data is:\n", data.head())

X = data.iloc[:, :-1]
print("\nThe First 5 values of the train data is\n", X.head())

y = data.iloc[:, -1]
print("\nThe First 5 values of train output is\n", y.head())

le = LabelEncoder()

for column in X.columns:
    X[column] = le.fit_transform(X[column])

print("\nNow the Train input is\n", X.head())

y = le.fit_transform(y)
print("\nNow the Train output is\n", y)

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.20)

classifier = GaussianNB()
classifier.fit(X_train, y_train)

print("Accuracy is:", accuracy_score(classifier.predict(X_test), y_test))
