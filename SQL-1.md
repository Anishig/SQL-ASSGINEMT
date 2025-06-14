```sql
1.
SELECT 
    P.PARTY_ID,
    P.CREATED_DATE AS ENTRY_DATE,
    PE.FIRST_NAME,
    PE.LAST_NAME,
    CM.INFO_STRING,
    TN.CONTACT_NUMBER
FROM PARTY AS P
JOIN PARTY_CONTACT_MECH AS PCM ON PCM.PARTY_ID = P.PARTY_ID
JOIN PERSON AS PE ON PE.PARTY_ID = P.PARTY_ID
LEFT JOIN CONTACT_MECH AS CM ON CM.CONTACT_MECH_ID = PCM.CONTACT_MECH_ID
LEFT JOIN TELECOM_NUMBER AS TN ON TN.CONTACT_MECH_ID = CM.CONTACT_MECH_ID;

2.
SELECT 
    P.PRODUCT_ID,
    P.PRODUCT_TYPE_ID,
    P.INTERNAL_NAME
FROM PRODUCT AS P
JOIN PRODUCT_TYPE AS PT ON P.PRODUCT_TYPE_ID = PT.PRODUCT_TYPE_ID
WHERE PT.IS_PHYSICAL = 'Y';

3.
SELECT 
    P.PRODUCT_ID,
    P.INTERNAL_NAME,
    P.PRODUCT_TYPE_ID,
    GI.ID_VALUE AS NETSUITE_ID
FROM PRODUCT AS P
JOIN GOOD_IDENTIFICATION AS GI ON GI.PRODUCT_ID = P.PRODUCT_ID
WHERE GI.GOOD_IDENTIFICATION_TYPE_ID = 'ERP_ID';

4.
SELECT 
    P.PRODUCT_ID,
    G2.ID_VALUE AS SHOPIFY_ID,
    P.PRODUCT_ID AS HOTWAX_ID,
    GI.ID_VALUE AS NETSUITE_ID
FROM PRODUCT AS P
JOIN GOOD_IDENTIFICATION AS GI ON GI.PRODUCT_ID = P.PRODUCT_ID
JOIN GOOD_IDENTIFICATION AS G2 ON G2.PRODUCT_ID = P.PRODUCT_ID
WHERE GI.GOOD_IDENTIFICATION_TYPE_ID = 'ERP_ID'
  AND G2.GOOD_IDENTIFICATION_TYPE_ID = 'SHOPIFY_PROD_ID';

5.
SELECT 
    P.PRODUCT_ID,
    P.PRODUCT_TYPE_ID,
    OH.PRODUCT_STORE_ID,
    OI.QUANTITY,
    P.INTERNAL_NAME,
    P.FACILITY_ID,
    OH.EXTERNAL_ID,
    F.FACILITY_TYPE_ID,
    OHS.ORDER_HISTORY_ID,
    OI.ORDER_ID,
    OI.ORDER_ITEM_SEQ_ID,
    OI.SHIP_GROUP_SEQ_ID
FROM ORDER_HEADER AS OH
JOIN ORDER_ITEM AS OI ON OH.ORDER_ID = OI.ORDER_ID
JOIN PRODUCT AS P ON OI.PRODUCT_ID = P.PRODUCT_ID
JOIN ORDER_HISTORY AS OHS ON OHS.ORDER_ID = OH.ORDER_ID
LEFT JOIN FACILITY AS F ON P.FACILITY_ID = F.FACILITY_ID
JOIN ORDER_STATUS AS OS ON OS.ORDER_ID = OH.ORDER_ID
WHERE OS.STATUS_ID = 'ORDER_COMPLETED'
  AND OS.STATUS_DATETIME >= DATE_FORMAT(CURDATE() - INTERVAL 1 MONTH, '%Y-%m-01')
  AND OS.STATUS_DATETIME < DATE_FORMAT(CURDATE(), '%Y-%m-01');

7.
SELECT 
    OH.ORDER_ID,
    OH.GRAND_TOTAL AS TOTAL_AMOUNT,
    OH.EXTERNAL_ID AS SHOPIFY_ID,
    OPP.PAYMENT_METHOD_TYPE_ID AS PAYMENT_METHOD
FROM ORDER_HEADER AS OH
JOIN ORDER_PAYMENT_PREFERENCE AS OPP ON OH.ORDER_ID = OPP.ORDER_ID;

8.
SELECT 
    OH.ORDER_ID,
    OH.STATUS_ID AS ORDER_STATUS,
    OPP.STATUS_ID AS PAYMENT_STATUS,
    S.SHIPMENT_ID AS SHIPMENT_STATUS
FROM ORDER_HEADER AS OH
JOIN ORDER_PAYMENT_PREFERENCE AS OPP ON OH.ORDER_ID = OPP.ORDER_ID
JOIN ORDER_SHIPMENT AS S ON OH.ORDER_ID = S.ORDER_ID;

9.
SELECT 
    DATE_FORMAT(STATUS_DATETIME, '%Y-%m-%d %H:00:00') AS HOUR,
    COUNT(*) AS TOTAL_ORDERS
FROM ORDER_STATUS
WHERE STATUS_ID = 'ORDER_COMPLETED'
GROUP BY HOUR;

10.
SELECT 
    COUNT(DISTINCT OISG.ORDER_ID) AS TOTAL_ORDERS,
    SUM(OH.GRAND_TOTAL) AS TOTAL_REVENUE
FROM ORDER_ITEM_SHIP_GROUP AS OISG
LEFT JOIN ORDER_HEADER AS OH ON OISG.ORDER_ID = OH.ORDER_ID
WHERE OISG.SHIPMENT_METHOD_TYPE_ID = 'STOREPICKUP';

11.
SELECT 
    OS.STATUS_ID,
    COUNT(OH.ORDER_ID) AS TOTAL_ORDERS,
    OS.CHANGE_REASON
FROM ORDER_HEADER AS OH
JOIN ORDER_STATUS AS OS ON OS.ORDER_ID = OH.ORDER_ID
WHERE OS.STATUS_ID = 'ORDER_CANCELLED'
GROUP BY OS.STATUS_ID, OS.CHANGE_REASON;

12.
SELECT 
    II.PRODUCT_ID,
    (II.QUANTITY_ON_HAND_TOTAL - II.AVAILABLE_TO_PROMISE_TOTAL) AS THRESHOLD
FROM INVENTORY_ITEM AS II;
```
