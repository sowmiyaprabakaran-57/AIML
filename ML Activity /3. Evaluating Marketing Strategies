# Import necessary libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, classification_report
from sklearn.tree import plot_tree
import matplotlib.pyplot as plt

# Sample data for advertising strategies (replace with actual data)
data = {
    'ad_type': ['online', 'tv', 'online', 'tv', 'print', 'online', 'tv', 'print', 'online', 'print'],
    'budget': [1000, 1500, 800, 1200, 700, 900, 1400, 750, 1100, 850],
    'target_age': [25, 35, 22, 30, 40, 27, 32, 45, 23, 38],
    'outcome': [1, 1, 0, 1, 0, 0, 1, 0, 1, 0]  # 1 for success, 0 for no success
}

# Convert categorical variable to dummy variables
df = pd.DataFrame(data)
df = pd.get_dummies(df, columns=['ad_type'], drop_first=True)

# Define features (X) and target variable (y)
X = df.drop('outcome', axis=1)
y = df['outcome']

# Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Initialize and train the Decision Tree model
model = DecisionTreeClassifier(random_state=42)
model.fit(X_train, y_train)

# Predict on test data
y_pred = model.predict(X_test)

# Evaluate model performance
accuracy = accuracy_score(y_test, y_pred)
precision = precision_score(y_test, y_pred)
recall = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)

print("Accuracy:", accuracy)
print("Precision:", precision)
print("Recall:", recall)
print("F1-Score:", f1)
print("\nClassification Report:\n", classification_report(y_test, y_pred))

# Feature importance
print("\nFeature Importances:", model.feature_importances_)

# Visualize the Decision Tree
plt.figure(figsize=(12, 8))
plot_tree(model, feature_names=X.columns, class_names=['No Success', 'Success'], filled=True)
plt.show()
