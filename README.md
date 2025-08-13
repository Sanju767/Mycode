from collections import defaultdict

# Reverse mapping from your product_category_map (Product → Category) 
# to Category → [Products]
category_to_products = defaultdict(list)
for product, category in product_category_map.items():
    category_to_products[category].append(product)

with col33:
    # Category dropdown with default = "Loans"
    categories = list(category_to_products.keys())
    default_category_index = categories.index("Loans") if "Loans" in categories else 0
    selected_category = st.selectbox(
        "Select Category",
        options=categories,
        index=default_category_index
    )

with col331:
    # Product dropdown with default = "Auto Loans"
    products_for_category = category_to_products[selected_category]
    default_product_index = products_for_category.index("Auto Loans") if "Auto Loans" in products_for_category else 0
    product_category = st.selectbox(
        "Select Product",
        options=products_for_category,
        index=default_product_index
    )
