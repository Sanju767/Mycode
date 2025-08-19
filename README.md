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
