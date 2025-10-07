# Insert, Update and Delete operations manually for an Interactive Grid in Oracle APEX

- This PL/SQL block handles Insert, Update, and Delete operations manually for an Interactive Grid in Oracle APEX.
- It uses the :APEX$ROW_STATUS system variable to determine the current row action.

---


```sql function
DECLARE
   vSEQ NUMBER := S_CM_EXPORT_BILL_DETAIL.NEXTVAL; 
   vAMOUNT NUMBER;
BEGIN

   vAMOUNT := NVL(:BILL_QTY,0) * NVL(:UNIT_PRICE,0);

   CASE :APEX$ROW_STATUS
      WHEN 'C' THEN
         INSERT INTO CM_EXPORT_BILL_DETAIL (
            EXPORT_BILL_DETAIL_ID,
            EXPORT_BILL_ID,
            PI_DETAIL_ID,
            ITEM_NO,
            BILL_QTY,
            QTY_LBS,
            NO_OF_BAG,
            WEIGHT,
            UNIT_PRICE,
            UNIT_PRICE_LBS,
            AMOUNT,
            ORDER_NO,
            COMPANY_ID,
            UNIT_ID,
            CREATED_DT,
            CREATED_BY
         ) VALUES (
            vSEQ,
            :P26_EXPORT_BILL_ID,
            :PI_DETAIL_ID,
            :ITEM_NO,
            :BILL_QTY,
            :QTY_LBS,
            :NO_OF_BAG,
            :WEIGHT,
            :UNIT_PRICE,
            :UNIT_PRICE_LBS,
            vAMOUNT,
            :ORDER_NO,
            :COMPANY_ID,
            :UNIT_ID,
            SYSDATE,
            :APP_USER
         )
         RETURNING EXPORT_BILL_DETAIL_ID INTO :EXPORT_BILL_DETAIL_ID;

      WHEN 'U' THEN
         UPDATE CM_EXPORT_BILL_DETAIL
            SET PI_DETAIL_ID   = :PI_DETAIL_ID,
                ITEM_NO        = :ITEM_NO,
                BILL_QTY       = :BILL_QTY,
                QTY_LBS        = :QTY_LBS,
                NO_OF_BAG      = :NO_OF_BAG,
                WEIGHT         = :WEIGHT,
                UNIT_PRICE     = :UNIT_PRICE,
                UNIT_PRICE_LBS = :UNIT_PRICE_LBS,
                AMOUNT         = vAMOUNT,   
                ORDER_NO       = :ORDER_NO,
                COMPANY_ID     = :COMPANY_ID,
                UNIT_ID        = :UNIT_ID,
                UPDATE_DT      = SYSDATE,
                UPDATED_BY     = :APP_USER
          WHERE EXPORT_BILL_DETAIL_ID = :EXPORT_BILL_DETAIL_ID;

      WHEN 'D' THEN
         DELETE FROM CM_EXPORT_BILL_DETAIL
          WHERE EXPORT_BILL_DETAIL_ID = :EXPORT_BILL_DETAIL_ID;
   END CASE;
END;


```

 # Thank you
 ## Sanjay Sikder

 You can connect with me on [LinkedIn](https://www.linkedin.com/in/sanjay-sikder/)!
