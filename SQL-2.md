```sql

5.1.

SELECT OH.ORDER_ID,ORL.PARTY_ID ,P.FIRST_NAME,P.LAST_NAME ,PA.POSTAL_CODE,PA.CITY,OS.STATUS_ID,OH.ORDER_DATE  FROM ORDER_HEADER AS OH
JOIN ORDER_ROLE AS ORL ON OH.ORDER_ID=ORL.ORDER_ID
JOIN PERSON AS P ON P.PARTY_ID=ORL.PARTY_ID
JOIN PARTY_CONTACT_MECH AS PCM ON PCM.PARTY_ID=ORL.PARTY_ID
LEFT JOIN CONTACT_MECH AS CM ON PCM.CONTACT_MECH_ID=CM.CONTACT_MECH_ID
LEFT JOIN POSTAL_ADDRESS AS PA ON PA.CONTACT_MECH_ID=CM.CONTACT_MECH_ID
LEFT JOIN ORDER_STATUS AS OS ON OH.ORDER_ID=OS.ORDER_ID
WHERE OS.STATUS_DATETIME>='2023-10-01' AND OS.STATUS_DATETIME<'2023-11-01' AND
CM.CONTACT_MECH_TYPE_ID='POSTAL_ADDRESS';


5.2.

SELECT  OH.ORDER_ID,OH.ORDER_DATE,OH.STATUS_ID ,PE.FIRST_NAME,LAST_NAME ,PA.ADDRESS1 ,PA.CITY , PA.STATE_PROVINCE_GEO_ID,PA.COUNTRY_GEO_ID AS COUNTRY ,PA.POSTAL_CODE FROM ORDER_HEADER AS OH
JOIN ORDER_ROLE AS ORL ON OH.ORDER_ID=ORL.ORDER_ID
JOIN PERSON AS PE ON ORL.PARTY_ID=PE.PARTY_ID
LEFT JOIN PARTY_CONTACT_MECH AS PCM ON PCM.PARTY_ID=ORL.PARTY_ID
LEFT JOIN CONTACT_MECH AS CM ON CM.CONTACT_MECH_ID=PCM.CONTACT_MECH_ID
LEFT JOIN POSTAL_ADDRESS AS PA ON PA.CONTACT_MECH_ID=CM.CONTACT_MECH_ID
WHERE CM.CONTACT_MECH_TYPE_ID='POSTAL_ADDRESS' 
AND PA.STATE_PROVINCE_GEO_ID='NY'

5.3.

SELECT OI.PRODUCT_ID, P.INTERNAL_NAME, SUM(QUANTITY) AS TOTAL_QUANTITY_SOLD, PA.CITY,PA.STATE_PROVINCE_GEO_ID,SUM(QUANTITY*UNIT_PRICE) AS REVENUE
FROM ORDER_ITEM  AS OI
LEFT JOIN PRODUCT AS P ON P.PRODUCT_ID=OI.PRODUCT_ID
LEFT JOIN ORDER_ROLE AS ORL ON ORL.ORDER_ID=OI.ORDER_ID
LEFT JOIN PARTY_CONTACT_MECH AS PCM ON PCM.PARTY_ID=ORL.PARTY_ID
LEFT JOIN POSTAL_ADDRESS AS PA ON PCM.CONTACT_MECH_ID=PA.CONTACT_MECH_ID
WHERE PA.STATE_PROVINCE_GEO_ID='NY'
GROUP BY OI.PRODUCT_ID ORDER BY REVENUE DESC;

7.3

SELECT F.FACILITY_ID,F.FACILITY_NAME, COUNT(DISTINCT OI.ORDER_ID) AS TOTAL_ORDERS,SUM(OI.QUANTITY*OI.UNIT_PRICE) AS TOTAL_REVENUE FROM ORDER_ITEM_SHIP_GROUP AS OISG
LEFT JOIN FACILITY AS F ON OISG.FACILITY_ID=F.FACILITY_ID
JOIN ORDER_ITEM AS OI ON OI.ORDER_ID=OISG.ORDER_ID AND OI.SHIP_GROUP_SEQ_ID=OISG.SHIP_GROUP_SEQ_ID
GROUP BY OISG.FACILITY_ID;



8.1
SELECT II.INVENTORY_ITEM_ID ,PRODUCT_ID ,FACILITY_ID ,IID.REASON_ENUM_ID FROM  INVENTORY_ITEM AS II 
LEFT JOIN INVENTORY_ITEM_DETAIL AS IID  ON IID.INVENTORY_ITEM_ID = II.INVENTORY_ITEM_ID
where IID.REASON_ENUM_ID in ('VAR_DAMAGED','VAR_LOST');


8.2
DOUBTFUL


8.3

SELECT OH.ORDER_ID,OS.STATUS_ID ,F.FACILITY_ID, F.FACILITY_NAME,F.FACILITY_TYPE_ID FROM ORDER_HEADER AS OH 
JOIN ORDER_STATUS AS OS ON OH.ORDER_ID=OS.ORDER_ID
JOIN ORDER_ITEM_SHIP_GROUP AS OISG ON OISG.ORDER_ID=OH.ORDER_ID
JOIN FACILITY AS F ON F.FACILITY_ID=OISG.FACILITY_ID
WHERE OS.STATUS_ID='ORDER_CREATED';


8.4

SELECT PF.PRODUCT_ID,PF.FACILITY_ID,II.QUANTITY_ON_HAND_TOTAL,II.AVAILABLE_TO_PROMISE_TOTAL ,II.QUANTITY_ON_HAND_TOTAL-II.AVAILABLE_TO_PROMISE_TOTAL AS DIFFERNECE FROM PRODUCT_FACILITY AS PF
JOIN INVENTORY_ITEM AS II ON II.PRODUCT_ID=PF.PRODUCT_ID


8.5

SELECT OS.ORDER_ID,OS.ORDER_ITEM_SEQ_ID,OS.STATUS_ID AS CURRENT_STATUS_ID,OS.STATUS_DATETIME AS STATUS_CHANGE_DATETIME,OS.STATUS_USER_LOGIN AS CHANGED_BY FROM ORDER_STATUS AS OS



8.6

SELECT OH.SALES_CHANNEL_ENUM_ID AS SALES_CHANNEL,COUNT(OH.ORDER_ID) AS TOTAL_ORDERS,SUM(OI.QUANTITY*OI.UNIT_PRICE)  AS TOTAL_REVENUE FROM ORDER_HEADER AS OH
JOIN ORDER_ITEM AS OI ON OI.ORDER_ID=OH.ORDER_ID
GROUP BY OH.SALES_CHANNEL_ENUM_ID

? reporting period not found IN Q 8.6.

```
