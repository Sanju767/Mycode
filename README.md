proc sql;
    create table enriched_data as
    select 
        a.*,

        /* Segment mapping (raw → name) */
        case a.segment
            when 'PE' then 'Premier Executive'
            when 'V'  then 'Premier'
            when 'WP' then 'Wealth Premier'
            when 'P'  then 'Advance'
            when 'W'  then 'Personal Banking'
            when 'DR' then 'Other RBWM'
            when 'PS' then 'Sole Props Advance'
            when 'VS' then 'Sole Props Premier'
            when 'MS' then 'Sole Props Mass'
            when 'A'  then 'Private Client Services'
            when 'PB' then 'Private Banking'
            when 'M'  then 'Municipios Retail Business Banking'
            when 'DP' then 'Other GPB'
            else 'Other'
        end as segment_name,

        /* Segment category mapping */
        case 
            when a.segment in ('PE','V','WP') then 'Premier'
            when a.segment = 'P'              then 'Advance'
            when a.segment = 'W'              then 'Personal Banking'
            when a.segment in ('DR')          then 'Other'
            when a.segment in ('PS','VS','MS','M') then 'RBB'
            when a.segment in ('A','PB','DP') then 'GPB'
            else 'Other'
        end as segment_category,

        /* Product flags */
        case when a.ID_Product = 'TMD' then 1 else 0 end as has_TMD,
        case when a.ID_Product = 'Mortgages' then 1 else 0 end as has_Mortgage,
        case when a.ID_Product in ('Mortgages','Credit Cards','Payroll Loans','Personal Loans','Auto Loans')
             then 1 else 0 end as has_Loans

    from raw_data as a

    /* Filtering */
    where a.flag_active_inactive = 1
      and calculated segment_category in ('Premier','Advance','Personal Banking');
quit;


# Ensure 'Segment' column keeps your defined order
segment_order = ['Premier', 'Advance', 'Personal Banking', 'GPB', 'RBB', 'Other']
segment_line['Segment'] = pd.Categorical(segment_line['Segment'], 
                                         categories=segment_order, 
                                         ordered=True)


                                         




color_map = {
    "Credit Cards": "#1f77b4",       # Blue
    "Payroll Loans": "#ff7f0e",      # Orange
    "Personal Loans": "#2ca02c",     # Green
    "Mortgages": "#d62728",          # Red
    "Auto Loans": "#9467bd",         # Purple
    "Kavak": "#8c564b",              # Brown
    "Installment": "#e377c2",        # Pink
    "Lombard Lending": "#7f7f7f",    # Gray
    "Revolving": "#bcbd22",          # Olive
    "CSI": "#17becf",                # Cyan
    "Real Estate Ban": "#ff9896",    # Light Red
    "Time Deposits": "#c5b0d5",      # Lavender
    "Demand Deposits": "#98df8a",    # Light Green
    "Savings": "#aec7e8",            # Light Blue
    "Investment DPM": "#ffbb78",     # Light Orange
    "Investment Fund": "#f7b6d2",    # Rose
    "Structured Note": "#c49c94",    # Beige
    "Debit Cards": "#9edae5",        # Aqua
    "Investment Repo": "#dbdb8




    






import streamlit as st
import plotly.express as px
import pandas as pd

# Fixed color mapping for your products
color_map = {
    "Credit Cards": "#1f77b4",       # Blue
    "Payroll Loans": "#ff7f0e",      # Orange
    "Personal Loans": "#2ca02c",     # Green
    "Mortgages": "#d62728",          # Red
    "Auto Loans": "#9467bd",         # Purple
    "Kavak": "#8c564b",              # Brown
    "Installment": "#e377c2",        # Pink
    "Lombard Lending": "#7f7f7f",    # Gray
    "Revolving": "#bcbd22",          # Olive
    "CSI": "#17becf",                # Cyan
    "Real Estate Ban": "#ff9896",    # Light Red
    "Time Deposits": "#c5b0d5",      # Lavender
    "Demand Deposits": "#98df8a",    # Light Green
    "Savings": "#aec7e8",            # Light Blue
    "Investment DPM": "#ffbb78",     # Light Orange
    "Investment Fund": "#f7b6d2",    # Rose
    "Structured Note": "#c49c94",    # Beige
    "Debit Cards": "#9edae5",        # Aqua
    "Investment Repo": "#dbdb8d",    # Light Olive
    "Capitales": "#393b79",          # Dark Blue
    "Custody": "#637939",            # Dark Green
    "Insurance": "#8c6d31",          # Dark Brown
    "Insurance Auto": "#843c39",     # Dark Red
    "Insurance Morg": "#7b4173",     # Dark Purple
    "Trading": "#5254a3"             # Indigo
}

# Example dataset
df = pd.DataFrame({
    "Product": ["Credit Cards", "Payroll Loans", "Personal Loans", "Trading"],
    "Value": [120, 80, 150, 200]
})

# Plot with consistent colors
fig = px.bar(
    df,
    x="Value",
    y="Product",
    orientation="h",
    color="Product",
    color_discrete_map=color_map
)

st.plotly_chart(fig, use_container_width=True)
data expense_new1;
    set expense_202403;

    /* ---- Update VINTAGE classification ---- */
    if VINTAGE in ('0M-1M', '02M-12M') then VINTAGE = '0-12M';

    /* ---- Update PORTA_FLAG ---- */
    if PORTA_FLAG = 'NA' then PORTA_FLAG = 'Active';
    else if PORTA_FLAG = 'Porta Out' then PORTA_FLAG = 'porta out';
    else if PORTA_FLAG = 'Porta In' then PORTA_FLAG = 'porta in';
    else if PORTA_FLAG = '.' then PORTA_FLAG = 'No Porta';

    /* ---- Update flag_Payroll ---- */
    if flag_Payroll = 'EX Payroll' then flag_Payroll = 'Ex-Payroll';
    else if flag_Payroll = 'NO Payroll' then flag_Payroll = 'Non-Payroll';

run;


