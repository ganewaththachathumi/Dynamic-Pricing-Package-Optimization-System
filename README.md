# Dynamic-Pricing-Package-Optimization-System


## Table of Contents
- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Collection](#data-collection)
- [Data Cleaning](#data-cleaning)
- [Data Analysis](#data-analysis)
- [Results](#results)
- [Recommendations](#recommendations)
- [Limitations](#limitations)
- [Conclusion](#conclusion)

---

## Project Overview
This project is a **Flask-based Dynamic Pricing & Package Optimization System** that helps businesses **optimize service pricing and feature selection**. It dynamically calculates project costs, suggests suitable packages, and utilizes historical data for strategic decision-making.

### Key Features:
- 📌 **Smart Pricing Engine** – Dynamically adjusts project costs based on complexity & selected features.
- 📌 **Package Recommendations** – Suggests suitable packages if the budget is too low.
- 📌 **Historical Data Storage** – Saves user input in an Excel sheet for analysis.
- 📌 **Power BI Dashboard** – Provides insights to refine pricing and package decisions.

---

## Data Sources
- **User Inputs**: Project type, complexity, selected features, and budget.
- **Historical Data**: Stored user selections in an Excel sheet.
- **Predefined Packages**: Standard pricing for common service combinations.

---

## Tools
- **Backend**: Python (Flask)
- **Database**: Excel (for historical data storage)
- **Frontend**: HTML, CSS
- **Data Analysis & Visualization**: Power BI

---

## Data Collection
User inputs (project type, complexity, selected features, budget) are collected through a **Flask web interface** and stored in an Excel sheet for future analysis.

---

## Data Cleaning
Basic data validation is performed to ensure accurate user inputs. Duplicate or invalid entries are handled programmatically.

---

## Data Analysis
The stored historical data is analyzed in **Power BI** to generate insights such as:
- 📊 **Package selection trends over time**
- 💰 **Budget vs. selected packages**
- 🔥 **Most popular features**
- ⚙️ **Complexity level demand analysis**
- 📉 **Average project pricing & profitability**

**Python Code for Data Analysis:**
```python
from flask import Flask, render_template, request

app = Flask(__name__)

# Define the pricing structure
feature_prices = {
    'web_app': 50000,
    'mobile_app': 40000,
    'ecommerce': 80000,
    'payment_gateway': 20000,
    'social_login': 15000,
    'push_notifications': 10000,
    'multi_language': 12000,
    'analytics_dashboard': 25000,
}

# Complexity level price multipliers
complexity_multipliers = {
    1: 1.0,  # Low complexity - No extra cost
    2: 1.2,  # Medium complexity - 20% increase
    3: 1.4  # High complexity - 40% increase
}

# Predefined packages
packages = {
    "Basic Package": {
        "base_price": 50000,
        "features": ["Web/Mobile App", "Social Media Login"]
    },
    "Standard Package": {
        "base_price": 80000,
        "features": ["Web/Mobile App", "Social Login", "Push Notifications", "Multi-language Support"]
    },
    "Premium Package": {
        "base_price": 120000,
        "features": ["E-commerce Platform", "Payment Gateway", "Social Login", "Push Notifications", "Multi-language",
                     "Analytics Dashboard"]
    },
    "Custom Package": {
        "base_price": 150000,
        "features": ["Fully customizable website with all features"]
    }
}


@app.route('/', methods=['GET'])
def home():
    return render_template('web.html', total_price=None, suggestion=None, show_packages=False, packages=packages)


@app.route('/calculate_price', methods=['POST'])
def calculate_price():
    project_type = request.form['project_type']
    complexity = int(request.form['complexity'])
    features = request.form.getlist('features')
    budget = int(request.form['budget'])

    # Base price for selected project type
    base_price = feature_prices.get(project_type, 0)

    # Feature cost
    feature_cost = sum(feature_prices.get(feature, 0) for feature in features)

    # Apply complexity multiplier
    total_price = (base_price + feature_cost) * complexity_multipliers[complexity]

    # Determine suggestion and package display
    suggestion = "The price is within your budget."
    show_packages = False

    if total_price > budget:
        suggestion = "Your budget is too low. Consider these available packages:"
        show_packages = True  # Show packages when budget is too low

    # Apply complexity multiplier to predefined package prices
    for package in packages.values():
        package["price"] = package["base_price"] * complexity_multipliers[complexity]

    return render_template('web.html', total_price=total_price, suggestion=suggestion, show_packages=show_packages,
                           packages=packages)


@app.route('/select_package', methods=['POST'])
def select_package():
    selected_package = request.form['selected_package']

    # Display a notification that the package was selected
    return render_template('web.html', notification=f'{selected_package} selected!', total_price=None, suggestion=None,
                           show_packages=False, packages=packages)


if __name__ == '__main__':
    app.run(debug=True)
```

## Results
The system enables businesses to:
- Optimize service pricing.
- Suggest appropriate packages based on budget.
- Analyze customer preferences and trends.
- Adjust package offerings for better profitability.

---

## Recommendations
- Continuously update predefined package offerings based on insights.
- Implement an advanced **machine learning model** for smarter package recommendations.
- Expand database storage to handle larger datasets.

---

## Limitations
- Currently supports only **Excel storage** (No SQL database).
- Static pricing model (could be enhanced with dynamic demand-based pricing).
- No real-time user authentication system.

---

## Conclusion
This project provides a **data-driven approach** to pricing and package selection, helping businesses make informed decisions based on historical trends and customer preferences. 🚀

