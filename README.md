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
with col33:
    # Add "All Categories" option at the top
    categories = ["All Categories"] + list(product_category_map.keys())
    default_category_index = categories.index("loans") if "loans" in categories else 1
    selected_category = st.selectbox(
        "Select Category",
        options=categories,
        index=default_category_index
    )

with col331:
    if selected_category == "All Categories":
        # Only "All Products" option if all categories are selected
        products_for_category = ["All Products"]
    else:
        # Add "All Products" for specific category
        products_for_category = ["All Products"] + product_category_map[selected_category]
    
    default_product_index = (
        products_for_category.index("Auto Loans")
        if "Auto Loans" in products_for_category else 0
    )
    selected_product = st.selectbox(
        "Select Product",
        options=products_for_category,
        index=default_product_index
    )

# ---- Handling metrics logic ----
if selected_category == "All Categories":
    # Aggregate across all categories
    st.write("Showing sum of all category metrics...")
elif selected_product == "All Products":
    # Aggregate across all products in this category
    st.write(f"Showing sum of all products in category: {selected_category}")


else:
    # Filter to specific product
    st.write(f"Showing metrics for: {selected_product} in category {selected_st.markdown(
    "<h5 style='text-align: left; color: black;'>HSBC Mexico GP analysis R&L file</h5>",
    unsafe_allow_html=True
)
