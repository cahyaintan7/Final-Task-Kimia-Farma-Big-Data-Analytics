CREATE TABLE TABLE kimia_farma.kf_analysis AS
SELECT 
SELECT 
  t.transaction_id, 
  t.date, 
  t.branch_id, 
  kc.branch_name, 
  kc.kota, 
  kc.provinsi, 
  kc.rating AS rating_cabang, 
  t.customer_name, 
  t.product_id, 
  p.product_name, 
  p.price AS actual_price, 
  t.discount_percentage,
  CASE
    WHEN p.price <= 50000 then 0.10
    WHEN p.price > 50000 AND pd.price <= 100000 then 0.15
    WHEN p.price > 100000 AND pd.price <= 300000 then 0.2
    WHEN p.price > 300000 AND pd.price <= 500000 then 0.25
    ELSE 0.3      --p.price >500000 
    END AS persentase_gross_laba,
  (p.price * (1-tr.discount_percentage)) AS nett_sales,
  (p.price * (1 - tr.discount_percentage)) * CASE
    WHEN p.price <= 50000 THEN 0.10
    WHEN p.price > 50000 AND pd.price <= 100000 then 0.15
    WHEN p.price > 100000 AND pd.price <= 300000 then 0.2
    WHEN p.price > 300000 AND pd.price <= 500000 then 0.25
    ELSE 0.3   
    END AS nett_profit,
  t.rating AS rating_transaksi
FROM kimia_farma.kf_final_transaction AS t
  JOIN kimia_farma.kf_kantor_cabang AS kc 
  ON (t.branch_id = kc.branch_id)
  JOIN kimia_farma.kf_product AS p
  ON (t.product_id = p.product_id)
;
