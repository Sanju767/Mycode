# Mycode

import streamlit as st
from collections import defaultdict

# Your real mapping: Product → Category
product_to_category = {
    'Credit Cards': 'Loans',
    'Payroll Loans': 'Loans',
    'Personal Loans': 'Loans',
    'Mortgages': 'Loans',
    'Auto Loans': 'Loans',
    'Kavak': 'Loans',
    'Installment': 'Loans',
    'Lombard Lending': 'Loans',
    'Revolving': 'Loans',
    'CSI': 'Loans',
    'Real Estate Ban': 'Loans',
    'Time Deposits': 'Deposits',
    'Demand Deposits': 'Deposits',
    'Savings': 'Deposits',
    'Investment DPM': 'Investment',
    'Investment Fund': 'Investment',
    'Structured Note': 'Investment',
    'Debit Cards': 'Investment'
}

# Reverse mapping → Category : list of Products
category_to_products = defaultdict(list)
for product, category in product_to_category.items():
    category_to_products[category].append(product)

# 1. Select Category (default = "Loans")
categories = list(category_to_products.keys())
default_category_index = categories.index("Loans") if "Loans" in categories else 0
selected_category = st.selectbox(
    "Select Category",
    options=categories,
    index=default_category_index
)

# 2. Select Product (default = "Auto Loans")
products_for_category = category_to_products[selected_category]
default_product_index = products_for_category.index("Auto Loans") if "Auto Loans" in products_for_category else 0
selected_product = st.selectbox(
    "Select Product",
    options=products_for_category,
    index=default_product_index
)

st.write(f"Category: {selected_category}")
st.write(f"Product: {selected_product}")
