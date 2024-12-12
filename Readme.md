# **HMS**

# Stores Issue

## **1) File**

### a) Item View

- `Open`

```sql
--SP
EXECUTE S_STCON_POP_StoreMapping @FLAG=9,@HospID=1
-- Parameter
@HospID as Numeric,
@FLAG AS NUMERIC,
@PARAM1 AS VARCHAR(10)='',
@PARAM2 AS VARCHAR(50)=''
```

- `Search`

```sql
- SP
Exec S_ST_POP_ItemNameWithDetails 'Z'
-- Parameter
@SEARCHSTR as VARCHAR(150) =''
```

![alt text](./Stores%20Issue/image.png)

### b) Internal Indent

- `Open`

```SQL
-- SP
exec S_POP_ModuleID INVENTORY
-- Parameter
@modulename as varchar(50)
--SP
Exec S_OT_POP_OTStaff 1, 'IP INDENT AUTHORIZATION',59
-- Parameter
@HospId as numeric,
@LocationName as varchar(50),
@ModuleID as Numeric
--SP
Exec S_ST_POP_PurIndDepartments 0,1,1,6
-- Parameter
@isDeleted as integer=0,
@HospId as integer,
@OperatorId as numeric,
@Flag as int=1
```

- `Show`

```sql
-- SP
Exec S_ST_POP_PurindentList '12/11/2024 00:00:00 AM','12/11/2024 11:59:59 PM',1,313,2,0,0 --Store/Dept
Exec S_ST_POP_PurindentList '12/11/2024 00:00:00 AM','12/11/2024 11:59:59 PM',1,0,2,0,0 -- Authorized By

-- Parameter
@FromDate as datetime,
@ToDate as datetime,
@HospId as int,
@DeptId as int=0,
@Flag as int=1,
@IsDeleted as int=0,
@TypeID as int=0
```

![alt text](./Stores%20Issue/image-1.png)

- `Double Click` Or `View`

  ```sql
  EXEC S_SEC_ReturnPermissions 1,1,-1
  --SP
  Exec S_ST_ReturnDepartments 1,1, 'Can Raise Internal Indent'
  --SP
  Exec S_ST_POP_PurIndDepartments 0,1,1,5
  --SP
  Exec S_ST_POP_PurIndTransactions 0,3,1
  --SP
  Exec S_ST_ItemHelp 1,1
  --SP
  Exec dbo.S_ST_POP_IndentsMain  8644,1,0
  -- Parameter
  @IndentID as int,
  @HospID as int,
  @isDeleted as int=0
  --SP
  Exec S_ST_POP_DestinationStore 313,1,0
  ```

  ![alt text](./Stores%20Issue/image-44.png)

- Enter `Search Text`

  ![alt text](./Stores%20Issue/image-45.png)

  ```sql
  Exec S_ST_POP_ItemsInStoreByName 313,'m',1
  ```

- `New`

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
-- Parameter
@HOSPITALID   AS NUMERIC  ,
@USERID   AS NUMERIC ,
@MENUID   AS NUMERIC
Select Getdate() as SystemDate
-- SP
Exec S_ST_ReturnDepartments 1,1, 'Can Raise Internal Indent'
-- Parameter
@HospID as Numeric,
@OperatorID as Numeric,
@Characteristic as Varchar(100)
-- SP
Exec S_ST_POP_PurIndDepartments 0,1,1,5
-- Parameter
@isDeleted as integer=0,
@HospId as integer,
@OperatorId as numeric,
@Flag as int=1
-- SP
Exec S_ST_POP_PurIndTransactions 0,3,1
-- Parameter
@isDeleted as integer=0,
@Flag as int=1,
@HospID as int=0
-- SP
Exec S_ST_ItemHelp 1,1
-- Parameter
@Flag   AS NUMERIC,
@HospID  AS NUMERIC,
@Param1   AS NUMERIC = 0,
@Param2    AS NUMERIC=0,
@fromdate AS    DateTime='',
@todate AS    DateTime=''
```

![alt text](./Stores%20Issue/image-2.png)

- Select `Source Dept`

![alt text](./Stores%20Issue/image-3.png)

```sql
-- SP
Exec S_ST_POP_DestinationStore 395,1,0
-- Parameter
@isDeleted as integer=0,
@HospId as integer,
@OperatorId as numeric,
@Flag as int=1
```

- `Search`

![alt text](./Stores%20Issue/image-4.png)

```sql
-- SP
Exec S_ST_POP_ItemsInStoreByName 313,'a',1
-- Parameter
@IntStore as integer,
@SearchStr AS Varchar(150),
@intHospId as integer
```

- Select `Item name`

![alt text](./Stores%20Issue/image-5.png)

```sql
-- SP
Exec dbo.S_ST_InternalIndentsItemExistInStore  396,32,1
Select Getdate() as SystemDate
-- Parameter
@ItemID as Numeric,
@DeptID as Numeric,
@HospID as Numeric
```

- Enter `Req.Qty` like 5 or 10 & press `Save`

![alt text](./Stores%20Issue/image-46.png)

```sql
-- SP
Exec S_ST_IndentTransactions 2, '12/11/2024 12:17:38 PM' ,1
-- Parameter
set implicit_transactions on
Exec S_ST_SAVE_InternalIndentDetails '',1509,'I',313,2,'12/11/2024 12:17:38 PM','',0,1,1,37,32,0,0,'¤396¤10¤12/11/2024 12:20:04 PM¤£'
-- Parameter
@indentNo as varchar(25),
@catindenttype as int,
@purpose as char(1),
@DepartmentID as int,
@TransactionID as int,
@indentDate as datetime,
@Remarks as varchar(100),
@isdeleted as int=0,
@operatorID as int,
@HospID as int,
@TerminalID as int,
@DestDeptID as int,
@Typeid as int,
@DoctID as int=0,
@StrDetails as varchar(8000)=''

IF @@TRANCOUNT > 0 COMMIT TRAN

```

### c) Vendor Enquiry

![alt text](./Stores%20Issue/image-6.png)

- `Show`

```sql
-- Query
select * from t_sto_vendorenquiry tsv , t_sto_suppliermaster tsm where tsm.pk_supplierid = tsv.vendors and tsv.isdeleted = 0 and tsv.vdate between '07-02-2024 00:00:00' and '12-10-2024 23:59:59'
```

- `New`

```sql
-- SP
Exec S_ST_POP_Supplier 6,0,1
-- Parameter
@Mode    As  Int=0,
@Pk_SupplierID   As  Numeric=0,
@Hospid  As  Numeric=0   ,
@SupplierName  as varchar(50)=' '

SELECT ITEMNAME , PK_ITEMID FROM T_STO_ITEMMASTER WHERE ISDELETED = 0
Select Getdate() as SystemDate
```

![alt text](./Stores%20Issue/image-7.png)

- `Save`
  Enter `Search text` and press enter key

![alt text](./Stores%20Issue/image-47.png)

Press `Add to Enquiry List`

![alt text](./Stores%20Issue/image-48.png)

```sql
EXEC S_ST_SAVE_VENDORENQUIRYDETAILS 'BLOOD TEXT' , '12-11-2024' , '119' , 1 , 0
```

### d) Purchase Indent

```sql
-- SP
Exec S_ST_ReturnDepartments 1,1, 'Can Raise Purchase Indent'
-- Parameter
@HospID as Numeric,
@OperatorID as Numeric,
@Characteristic as Varchar(100)
```

![alt text](./Stores%20Issue/image-8.png)

- Select `Store` & press `Show` button

```sql
-- SP
Exec S_ST_POP_PurindentList '12/10/2024 00:00:00 AM','12/10/2024 11:59:59 PM',1,32,1,0,0
-- Parameter
@FromDate as datetime,
@ToDate as datetime,
@HospId as int,
@DeptId as int=0,
@Flag as int=1,
@IsDeleted as int=0,
@TypeID as int=0
```

- `New`

![alt text](./Stores%20Issue/image-9.png)

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
-- Parameter
@HOSPITALID   AS NUMERIC  ,
@USERID   AS NUMERIC ,
@MENUID   AS NUMERIC
```

```sql
-- SP
Exec S_ST_ReturnDepartments 1,1, 'Can Raise Purchase Indent'
-- Parameter
@HospID as Numeric,
@OperatorID as Numeric,
@Characteristic as Varchar(100)
```

```sql
-- SP
Exec S_ST_POP_PurIndTransactions 0,1,1
-- Parameter
@isDeleted as integer=0,
@Flag as int=1,
@HospID as int=0
```

```sql
-- SP
Exec S_ST_ItemHelp 1,1
-- Parameter
@Flag   AS NUMERIC,
@HospID  AS NUMERIC,
@Param1   AS NUMERIC = 0,
@Param2    AS NUMERIC=0,
@fromdate AS    DateTime='',
@todate AS    DateTime=''
```

```sql
-- SP
Exec S_ST_POP_PurIndTransactions 0,2,1
-- Parameter
@isDeleted as integer=0,
@Flag as int=1,
@HospID as int=0
```

- `Search Text`

![alt text](./Stores%20Issue/image-10.png)

```sql
-- SP
Exec S_ST_POP_ItemsInStoreByName 32,'m',1
-- Parameter
@IntStore as integer,
@SearchStr AS Varchar(150),
@intHospId as integer
```

- `Save`

![alt text](./Stores%20Issue/image-11.png)

```sql
--SP
Exec S_ST_IndentTransactions 9, '12/10/2024 4:39:11 PM' ,1
select * from Exec S_ST_IndentTransactions 9, '12/10/2024 4:39:11 PM' ,1
-- Parameter
@TransactiontypeID as int,
@Date as datetime,
@HospID as int

--SP
Exec S_ST_SAVE_PurIndDetails '',1510,'S',32,9,'12/11/2024 1:01:37 PM','',0,1,1,37,'¤268¤5¤12/31/2024 1:01:37 PM¤0¤0¤£'
-- Parameter
@indentNo as varchar(25),
@catindenttype as int,
@purpose as char(1),
@DepartmentID as int,
@TransactionID as int,
@indentDate as datetime,
@Remarks as varchar(100),
@isdeleted as int=0,
@operatorID as int,
@HospID as int,
@TerminalID as int,
@StrDetails as varchar(8000)=''


IF @@TRANCOUNT > 0 COMMIT TRAN
```

![alt text](./Stores%20Issue/image-49.png)

### e) Purchase Order

```sql
-- SP
exec S_POP_FormStatus 'PURCHASEORDER AMMENDMENT'
-- Parameter
@NAME AS VARCHAR(100) AS
-- SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=19,@HospID=1,@PARAM1=0,@PARAM2=0
-- Parameter
@Flag as Numeric,
@HospID as numeric,
@Param1  as  Numeric = 0,
@Param2  as  Numeric = 0,
@Param3  as  Numeric = 0
```

![alt text](./Stores%20Issue/image-12.png)

- Click `Show PO(s)` button

```sql
-- SP
EXECUTE S_ST_PurchaseOrder @HospID=1,@FromDate='12/1/2024',@ToDate='12/10/2024'
-- Parameter
@HospID as Numeric,
@FromDate as DateTime,
@ToDate as DateTime
```

- Click `New` button

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
-- Parameter
@HOSPITALID   AS NUMERIC  ,
@USERID   AS NUMERIC ,
@MENUID   AS NUMERIC
--SP
EXECUTE S_ST_DELETE_PurchaseOrderTemp  @HOSPID=1, @SystemID='EDPHMS'
-- Parameter
@HOSPID	as Numeric,
@SystemID as varchar(50)
-- SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=1,@HospID=1,@PARAM1=0,@PARAM2=0
-- Parameter
@Flag as Numeric,
@HospID as numeric,
@Param1  as  Numeric = 0,
@Param2  as  Numeric = 0,
@Param3  as  Numeric = 0
--SP
EXECUTE S_STCON_POP_StoreMapping @FLAG=9,@HospID=1
-- Parameter
@HospID as Numeric,
@FLAG AS NUMERIC,
@PARAM1 AS VARCHAR(10)='',
@PARAM2 AS VARCHAR(50)=''

--SP
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=1478
-- Parameter
@HospID as Numeric,
@ItemID as Numeric
```

![alt text](./Stores%20Issue/image-13.png)

- Select `Indent no` & press `Add`

Enter `Rate`, `quantity`, `discount`, `MRP` & `Tax Rate`

![alt text](./Stores%20Issue/image-50.png)

press `Save`

```sql
-- SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=3,@HospID=1,@PARAM1=0,@PARAM2=0

-- SP
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=1478

-- SP
EXECUTE S_ST_DELETE_PurchaseOrderDeliverySchedule  @FK_POID=2, @HOSPID=1, @SystemID='EDPHMS', @ItemID=268

-- SP
EXECUTE S_ST_SAVE_PurchaseOrderDeliverySchedule  @FK_POID=2, @PONo='PO/2/10', @Fk_ItemID=268, @FK_DepartmentID=32, @DeliveryDate='12/13/2024 3:35:25 PM', @Quantity=5, @SystemID='EDPHMS', @HospID=1, @TerminalID=37, @CreatedDate='12/11/2024 3:35:25 PM', @OperatorID=1

-- SP
EXECUTE S_ST_POP_NewTransactionNo  @TransactionID=10, @TransactionDate='12/11/2024', @HospID=1

-- SP
EXECUTE S_ST_UPDATE_RunningSeqNo  @TransactionID=10
-- Parameter
@TransactionID as Numeric

-- SP
EXECUTE S_ST_SAVE_PurchaseOrderMaster @PONo='PO/3/10', @POID=3, @FK_CatPOCreateTypeID=1500, @FK_SupplierID=119, @POdate='11/12/2024 3:35:25 PM', @FK_TransactionID=10, @POAmount=0, @FK_TermsConditions=0, @FK_POstatus=1505, @DiscountAmount=0, @CreatedDate='12/11/2024 3:35:25 PM', @OperatorID=1, @TerminalId=37, @IsDeleted=0, @Reason='', @TailID=0, @HospId=1, @Reference='', @Remarks='', @ModeofDispatch=''

-- SP
EXECUTE S_ST_SAVE_PurchaseOrderDetail @PONo='PO/3/10', @Fk_POID=3, @Fk_IndentID=8648, @IndentNo='PURIND/6/10', @FK_ItemID=268, @FK_PackingID=0, @Quantity=5, @UOM='6', @CostPrice=500, @SellingPrice=600, @TotalAmountOfItem=2425, @TotalTaxAmount=0, @TaxID=2, @IsFree=0, @FreeFor_ItemId=0, @DiscountAmount=75, @DiscountPercent=0, @HospID=1, @TerminalID=37, @IsDeleted=0, @CreatedDate='12/11/2024 3:35:25 PM', @TailID=0, @OperatorID=1

-- SP
EXECUTE S_ST_UPDATE_PurchaseOrderIndent  @IndentID=8648, @ItemID=268, @Quantity=5

-- SP
EXECUTE S_ST_UPDATE_PurchaseOrderPONo  @FK_POID=3, @PONo='PO/3/10', @HOSPID=1, @SystemID='EDPHMS'

```

![alt text](./Stores%20Issue/image-51.png)

- Clicke `Modify`

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
-- Parameter
@HOSPITALID   AS NUMERIC  ,
@USERID   AS NUMERIC ,
@MENUID   AS NUMERIC

-- SP
EXECUTE S_ST_DELETE_PurchaseOrderTemp  @HOSPID=1, @SystemID='EDPHMS'

-- SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=1,@HospID=1,@PARAM1=0,@PARAM2=0

-- SP
EXECUTE S_STCON_POP_StoreMapping @FLAG=9,@HospID=1

-- SP
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=1478

-- SP
EXECUTE S_ST_PurchaseOrderPoInfo @HospID=1,@PONo='PO/4/10'
-- Parameter
@HospID as Numeric,
@PONo as Varchar(50)

-- SP
exec S_ST_POP_RateContractDetails 119,1
-- SP
EXECUTE S_ST_SAVE_UomName @HospID=1,@UomID=6
-- Parameter
@HospID as Numeric,
@UomID as Numeric
```

![alt text](./Stores%20Issue/image-53.png)

Click `Modify` btn

```sql
--SP
EXECUTE S_ST_DELETE_PurchaseOrderDeliverySchedule  @FK_POID=4, @HOSPID=1, @SystemID='EDPHMS', @ItemID=268
--SP
EXECUTE S_ST_SAVE_PurchaseOrderDeliverySchedule  @FK_POID=4, @PONo='PO/4/10', @Fk_ItemID=268, @FK_DepartmentID=32, @DeliveryDate='12/13/2024 3:47:45 PM', @Quantity=5, @SystemID='EDPHMS', @HospID=1, @TerminalID=37, @CreatedDate='12/11/2024 3:47:44 PM', @OperatorID=1
--SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=14,@HospID=1,@PARAM1=268,@PARAM2=4
-- SP
EXECUTE S_ST_PurchaseOrderMaxTail @HospID=1,@POID=4
-- Parameter
@HospID as Numeric,
@POID as Numeric

EXECUTE S_ST_PurchaseOrderDeactivate  @HOSPID=1, @POID=4, @TailID=0
-- Parameter
@POID 		as Numeric,
@HOSPID		as Numeric,
@TailID			as Numeric

--SP
EXECUTE S_ST_SAVE_PurchaseOrderMaster @PONo='PO/4/10', @POID=4, @FK_CatPOCreateTypeID=1500, @FK_SupplierID=119, @POdate='11/12/2024 3:47:44 PM', @FK_TransactionID=10, @POAmount=2525, @FK_TermsConditions=0, @FK_POstatus=1505, @DiscountAmount=75, @CreatedDate='12/11/2024 3:47:44 PM', @OperatorID=1, @TerminalId=37, @IsDeleted=0, @Reason='', @TailID=1, @HospId=1, @Reference='', @Remarks='', @ModeofDispatch=''
--Parameter
@POID    as Numeric,
@PONo   as Varchar(50),
@FK_CatPOCreateTypeID as Numeric,
@FK_SupplierID   as Numeric,
@POdate   as Datetime,
@FK_TransactionID  as Numeric,
@POAmount   as Numeric(18,2),
@FK_TermsConditions  as Numeric,
@FK_POstatus   as Numeric,
@DiscountAmount  as Numeric(18,2),
@CreatedDate   as Datetime,
@OperatorID   as Numeric,
@TerminalId   as Numeric,
@IsDeleted   as Bit,
@Reason   as varchar(100),
@HospId   as Numeric,
@TailID    as Numeric,
@Reference as Varchar(2000),
@Remarks as Varchar(2000),
@ModeofDispatch   as Varchar(100)

--SP
EXECUTE S_ST_SAVE_PurchaseOrderDetail @PONo='PO/4/10', @Fk_POID=4, @Fk_IndentID=8648, @IndentNo='PURIND/6/10', @FK_ItemID=268, @FK_PackingID=0, @Quantity=5, @UOM='6', @CostPrice=500, @SellingPrice=600, @TotalAmountOfItem=2525, @TotalTaxAmount=100, @TaxID=2, @IsFree=0, @FreeFor_ItemId=0, @DiscountAmount=75, @DiscountPercent=0, @HospID=1, @TerminalID=37, @IsDeleted=0, @CreatedDate='12/11/2024 3:47:44 PM', @TailID=1, @OperatorID=1
-- Parameter
@Fk_POID   as Numeric,
@PONo   as Varchar(50),
@Fk_IndentID   as Numeric,
@IndentNo   as Varchar(50),
@FK_ItemID   as Numeric,
@FK_PackingID  as Numeric,
@Quantity   as Numeric(18,4),
@UOM    as Varchar(50),
@CostPrice   as Numeric(18,4),
@SellingPrice as Numeric(18,4) = 0 ,
@TotalAmountOfItem  as Numeric(18,4),
@TotalTaxAmount  as Numeric(18,4),
@TaxID as int ,
@IsFree   as Bit,
@FreeFor_ItemId  as Numeric,
@DiscountAmount  as Numeric(18,4),
@DiscountPercent  as Numeric(18,4),
@HospID   as Numeric,
@TerminalID   as Numeric,
@IsDeleted   as Bit,
@CreatedDate   as Datetime,
@OperatorID   as Numeric,
@TailID    as Numeric

--SP
EXECUTE S_ST_UPDATE_PurchaseOrderIndent  @IndentID=8648, @ItemID=268, @Quantity=0
-- Parameter
@IndentID		as Numeric,
@ItemId		as Numeric,
@Quantity		as Numeric(18,2)

--SP
EXECUTE S_ST_UPDATE_PurchaseOrderPONo  @FK_POID=4, @PONo='PO/4/10', @HOSPID=1, @SystemID='EDPHMS'
-- Parameter
@FK_POID 		as Numeric,
@PONo 		as Varchar(50),
@HOSPID		as Numeric,
@SystemID		as varchar(50)
```

![alt text](./Stores%20Issue/image-52.png)

- `Add Free`

![alt text](./Stores%20Issue/image-54.png)

Click `View All` or `Search Text`

```sql
EXECUTE S_STCON_POP_StoreMapping @FLAG=6,@HospID=1,@PARAM1=1,@PARAM2=0
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=1420
```

![alt text](./Stores%20Issue/image-55.png)

Select item & press `Add`

```sql
-- SP
EXECUTE S_ST_DELETE_PurchaseOrderTemp  @HOSPID=1, @SystemID='EDPHMS'
```

Then select `Supplier`

```sql
-- SP
exec S_ST_POP_RateContractDetails 119,1
```

![alt text](./Stores%20Issue/image-56.png)

- Change `PO Type`

```sql
-- SP
EXECUTE S_ST_DELETE_PurchaseOrderTemp  @HOSPID=1, @SystemID='EDPHMS'
-- Parameter
@HOSPID		as Numeric,
@SystemID		as varchar(50)
Exec S_ST_POP_ItemCodes
```

![alt text](./Stores%20Issue/image-14.png)

- Click `Add` button then active `Transaction Type`, `Supplier`, `Search Text`

- Select `Supplier`

```sql
-- SP
exec S_ST_POP_RateContractDetails 1,1
-- Parameter
@Suppliername as numeric,
@HospId as numeric
```

- `Search Text`

```sql
--SP
Exec S_ST_POP_ItemNames 'EN'
-- Parameter
 @SEARCHSTR as VARCHAR(150)
```

![alt text](./Stores%20Issue/image-15.png)

- `Save`

```sql
-- SP
EXECUTE S_ST_DELETE_PurchaseOrderTemp  @HOSPID=1, @SystemID='EDPHMS' --If not selected any itme
-- Parameter
@HOSPID		as Numeric,
@SystemID		as varchar(50)

-- SP
EXECUTE S_ST_DELETE_PurchaseOrderDeliverySchedule  @FK_POID=0, @HOSPID=1, @SystemID='EDPHMS', @ItemID=529
-- Parameter
@FK_POID 		as Numeric,
@HOSPID		as Numeric,
@SystemID		as varchar(50),
@ItemID		as Numeric

-- SP
EXECUTE S_ST_SAVE_PurchaseOrderDeliverySchedule  @FK_POID=0, @PONo='', @Fk_ItemID=529, @FK_DepartmentID=32, @DeliveryDate='12/13/2024 2:25:06 AM', @Quantity=5, @SystemID='EDPHMS', @HospID=1, @TerminalID=37, @CreatedDate='12/11/2024 2:25:05 AM', @OperatorID=1
-- Parameter
@FK_POID   as Numeric,
@PONo   as Varchar(50),
@Fk_ItemID  as Numeric,
@FK_DepartmentID as Numeric,
@DeliveryDate  as Datetime,
@Quantity  as Numeric(18,4),
@SystemID  as Varchar(50),
@HospID   as Numeric,
@TerminalID  as Numeric,
@CreatedDate  as Datetime,
@OperatorID  as Numeric

-- SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=14,@HospID=1,@PARAM1=529,@PARAM2=0
-- Parameter
@Flag as Numeric,
@HospID as numeric,
@Param1  as  Numeric = 0,
@Param2  as  Numeric = 0,
@Param3  as  Numeric = 0

EXECUTE S_ST_POP_NewTransactionNo  @TransactionID=10, @TransactionDate='12/28/2024', @HospID=1
-- Parameter
@TransactionID as Numeric,
@TransactionDate as Datetime,
@HospID as Numeric
```

- `Add Free` button & select the `Item` & `Drop down` & press `Add` button

![alt text](./Stores%20Issue/image-16.png)

```sql
-- SP
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=695
-- Parameter
@HospID as Numeric,
@ItemID as Numeric
```

- `View All`

```sql
-- SP
EXECUTE S_STCON_POP_StoreMapping @FLAG=6,@HospID=1,@PARAM1=1,@PARAM2=0
-- Parameter
@HospID as Numeric,
@FLAG AS NUMERIC,
@PARAM1 AS VARCHAR(10)='',
@PARAM2 AS VARCHAR(50)=''

--SP
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=754
-- Parameter
@HospID as Numeric,
@ItemID as Numeric
```

- `Calculate`

```sql
SELECT * FROM T_STO_TaxMaster WHERE PK_TAXID = 0
```

- `Save`

![alt text](./Stores%20Issue/image-17.png)

# Dought

```sql
SELECT * FROM T_STO_TaxMaster WHERE PK_TAXID = 0
```

### f) Goods Receipt Note(GRN)

- `Open`

```sql
-- SP
EXECUTE S_ST_POP_PurchaseOrder @FLAG=21,@HospID=1,@PARAM1=0,@PARAM2=0
-- Parameter
@Flag as Numeric,
@HospID as numeric,
@Param1  as  Numeric = 0,
@Param2  as  Numeric = 0,
@Param3  as  Numeric = 0
```

![alt text](./Stores%20Issue/image-18.png)

- Click `Show GRN(S)`

```sql
-- SP
EXECUTE S_ST_Srv @HospID=1,@FromDate='4/1/2024',@ToDate='12/11/2024'
-- Parameter
@HospID as Numeric,
@FromDate as DateTime,
@ToDate as DateTime
```

1. #### `View` btn

   ```sql
   -- SP
   EXEC S_SEC_ReturnPermissions 1,1,-1
   -- SP
   EXECUTE S_STCON_POP_StoreMapping @FLAG=9,@HospID=1
   -- SP
   EXECUTE S_ST_POP_PurchaseOrder @FLAG=22,@HospID=1,@PARAM1=0,@PARAM2=0
   -- Parameter
   @Flag as Numeric,
   @HospID as numeric,
   @Param1  as  Numeric = 0,
   @Param2  as  Numeric = 0,
   @Param3  as  Numeric = 0

   -- SP
   Exec S_ADM_FeeTypeMaster -1,0,1
   --Parameter
   @Pk_FeeTypeID as int=-1,
   @IsDeleted as int =2,
   @Hospid as numeric=-1

   Select Getdate() as SystemDate

   --SP
   Exec S_ST_ReturnDepartments @HospID=1,@OperatorID=1 ,@Characteristic='CAN RECEIVE SRV'

   -- SP
   Exec S_ST_POP_Supplier 6,0,1
   --Parameter
   @Mode    As  Int=0,
   @Pk_SupplierID   As  Numeric=0,
   @Hospid  As  Numeric=0   ,
   @SupplierName  as varchar(50)=' '

   Exec S_ST_POP_Srv 5,1,13,0,'', 0
   -- Parameter
   @Flag  As Int=0,
   @HospID As Numeric=0,
   @ID  As  Numeric=0,
   @ID1  As Numeric=0,
   @ID2  AS VARCHAR(25)='',
   @isfree as numeric = 0
   ```

   ![alt text](./Stores%20Issue/image-19.png)

   1. #### `View` btn

      ```sql
      -- SP
      Exec S_ST_ItemHelp 7,1,0,0,NULL,NULL
      -- Parameter

      ```

      ![alt text](./Stores%20Issue/image-20.png)

      - Click `Show`
      - Take loading alwase

      ```sql
      -- SP
      Exec S_ST_ItemHelp 6,1,0,0,NULL,NULL
      Exec S_STCON_POP_Masters  'T_STO_SupplierMaster','SupplierName','PK_SupplierID','1'
      -- Parameter
       @TableName    As  Varchar(50),
       @Select_Columns  As Varchar(100),
       @ID_ColumnName  As varchar(50),
       @Pk_ID    As  varchar(20)
      Exec S_ST_POP_Srv 11,1,0,0,'', 0
      -- Parameter
      ```

      - Select `Supplier Name` & click `Show`

      ```sql
      -- SP
      Exec S_ST_ItemHelp 9,1,119,0,NULL,NULL
      Exec S_STCON_POP_Masters  'T_STO_ItemMaster','ITEMname','pk_ITEMid','1405'
      -- Parameter
       @TableName    As  Varchar(50),
       @Select_Columns  As Varchar(100),
       @ID_ColumnName  As varchar(50),
       @Pk_ID    As  varchar(20)
      ```

      ![alt text](./Stores%20Issue/image-21.png)

2. #### `New` btn

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
EXECUTE S_STCON_POP_StoreMapping @FLAG=9,@HospID=1
EXECUTE S_ST_POP_PurchaseOrder @FLAG=22,@HospID=1,@PARAM1=0,@PARAM2=0
Exec S_ADM_FeeTypeMaster -1,0,1
Exec S_ST_ReturnDepartments @HospID=1,@OperatorID=1 ,@Characteristic='CAN RECEIVE SRV'
Exec S_ST_POP_Supplier 6,0,1
Exec S_ST_POP_Srv 5,1,13,0,'', 0
-- Parameter
@Flag  As Int=0,
@HospID As Numeric=0,
@ID  As  Numeric=0,
@ID1  As Numeric=0,
@ID2  AS VARCHAR(25)='',
@isfree as numeric = 0
```

![alt text](./Stores%20Issue/image-22.png)

- `Search Text`

![alt text](./Stores%20Issue/image-23.png)

```sql
-- SP
Exec S_ST_POP_ItemNames 'M'
-- Parameter
@SEARCHSTR as VARCHAR(150)
```

Double click on `Item Name` to add

```sql
Exec S_ST_POP_Srv 20,1,178,32,'', 0
```

![alt text](./Stores%20Issue/image-57.png)

Fill the all details

![alt text](./Stores%20Issue/image-58.png)

Press `Calculate` btn

```sql
SELECT * FROM T_STO_TaxMaster WHERE PK_TAXID = 2
```

![alt text](./Stores%20Issue/image-59.png)

# Dought

- Click `Add` button

```sql
-- SP
SELECT * FROM T_STO_TaxMaster WHERE PK_TAXID = 0
-- Parameter

```

### g) Purchase Result

![alt text](./Stores%20Issue/image-24.png)

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1

-- SP
EXECUTE S_ST_POP_Suppliers @HospID=1,@SupplierType=0
-- Parameter
@HospID as Numeric,
@SupplierType as Numeric

-- SP
EXECUTE S_ST_POP_PurchaseReturn @FLAG=1,@HospID=1,@PARAM1=0,@PARAM2=0
-- Parameter
@Flag as Numeric,
@HospID as numeric,
@Param1  as  Numeric = 0,
@Param2  as  Numeric = 0

-- SP
EXECUTE S_ST_POP_Stores @HospID=1
-- Parameter
@HospID as Numeric
```

- Click `Clear` then click `Add` btn
  Select `Supplier`
  Select `Store`

```sql
EXECUTE S_ST_POP_PurchaseReturn @FLAG=2,@HospID=1,@PARAM1=32,@PARAM2=119
```

Select `SRV No`

```sql
EXECUTE S_ST_POP_PurchaseReturn @FLAG=3,@HospID=1,@PARAM1=1893,@PARAM2=0
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=1405
EXECUTE S_ST_SAVE_ItemName @HospID=1,@ItemID=0
```

Select `item`
Click `Save`
![alt text](./Stores%20Issue/image-60.png)

```sql
IF @@TRANCOUNT > 0 ROLLBACK TRAN
set implicit_transactions off
EXECUTE S_ST_POP_NewTransactionNo  @TransactionID=11, @TransactionDate='12/11/2024', @HospID=1
set implicit_transactions on
EXECUTE S_ST_POP_PurchaseReturn @FLAG=4,@HospID=1,@PARAM1=0,@PARAM2=0
EXECUTE S_ST_UPDATE_RunningSeqNo  @TransactionID=11
EXECUTE S_ST_SAVE_PurchaseReturnMaster  @Pk_PurchaseReturnID=1, @PurchaseReturnNo='PURRTN/1/10', @PurchaseReturnDate='12/11/2024', @Fk_TransactionID=11, @Fk_DepartmentID=32, @Fk_SupplierID=119, @Fk_SRVID=1893, @SRVNo='SRV/190205001', @DeliveryChallanNO='', @DeliveryChallanDate='2/5/2019 10:13:30 AM', @HospId=1, @TerminalId=37, @IsDeleted=0, @CreatedDate='12/11/2024 5:29:33 PM', @OperatorID=1, @TailID=0
EXECUTE S_ST_UPDATE_STOCKLEDGERFORPURRET   @FromDepartmentID=32, @BinID=0, @ItemID=1405, @BatchNo='', @IssueQty=2, @PackOf=1, @ExpiryDate=NULL, @CostPrice=1100, @SellingPrice=0, @HospId=1, @TerminalId=37, @OperatorID=1, @TransactionID=11, @TransactionNo='PURRTN/1/10'
select * from EXECUTE S_ST_UPDATE_STOCKLEDGERFORPURRET   @FromDepartmentID=32, @BinID=0, @ItemID=1405, @BatchNo='', @IssueQty=2, @PackOf=1, @ExpiryDate=NULL, @CostPrice=1100, @SellingPrice=0, @HospId=1, @TerminalId=37, @OperatorID=1, @TransactionID=11, @TransactionNo='PURRTN/1/10'
IF @@TRANCOUNT > 0 ROLLBACK TRAN
```

### h) Purchase Result Track

![alt text](./Stores%20Issue/image-25.png)

```sql
SELECT PK_PURCHASERETURNID , PURCHASERETURNNO FROM T_STO_PURCHASERETURNS WHERE REPLACETRACKED = 0
select * from  SELECT PK_PURCHASERETURNID , PURCHASERETURNNO FROM T_STO_PURCHASERETURNS WHERE REPLACETRACKED = 0
```

### i) Issues

![alt text](./Stores%20Issue/image-26.png)

**SP**

```sql
-- SP
Exec S_ST_ReturnDepartments 1,1, 'Can Raise Internal Indent'
```

- Click `Modify` button

![alt text](./Stores%20Issue/image-27.png)

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
-- SP
Exec S_ST_POP_PurIndDepartments 0,1,1,3
-- SP
Exec S_ST_POP_PurIndTransactions 0,5,1
```

### j) Issue Acknowledgent

![alt text](./Stores%20Issue/image-28.png)

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
-- SP
Exec S_ST_POP_PurIndTransactions 0,5,1
-- SP
Exec S_ST_POP_PurIndDepartments 0,1,1,1
```

### k) Returns

- `Open`

```sql
-- SP
EXEC S_SEC_ReturnPermissions 1,1,-1
Exec S_ST_POP_PurIndDepartments 0,1,1,1
Exec S_ST_POP_PurIndTransactions 0,6,1
```

![alt text](./Stores%20Issue/image-29.png)

- Select `Indented Store/Dept`
  ![alt text](./Stores%20Issue/image-30.png)

```sql
-- SP
Exec S_ST_POP_IndentsForStore 0,1,1,3,313,0,0
-- Parameter
@StoreId as int,
@isDeleted as int,
@HospID as int,
@Flag as int=1,
@Tostore as Int=0,
@TypeId as int,
@IndentID as numeric=0

-- SP
Exec S_ST_POP_Stocks 1,313
-- Parameter
@intFlag as int=1,
@intStoreID as int=0
```

- Select `Issue No`
  ![alt text](./Stores%20Issue/image-31.png)

```sql
-- SP
Exec S_ST_POP_IssuedItems 8164,1,1
-- Parameter
```

- Click on `Save`

![alt text](./Stores%20Issue/image-32.png)

### l) Returns Acknowledgement

- `Open`

```sql
EXEC S_SEC_ReturnPermissions 1,1,-1
Exec S_ST_POP_PurIndDepartments 0,1,1,2
Exec S_ST_POP_PurIndTransactions 0,5,1
```

![alt text](./Stores%20Issue/image-33.png)

### m) Stock Report

- `Open`

```sql
EXEC S_SEC_ReturnPermissions 1,1,-1
Exec S_ST_POP_PurIndDepartments 0,1,1,3
```

![alt text](./Stores%20Issue/image-34.png)

- `Show`
  ![alt text](./Stores%20Issue/image-35.png)

```sql
Exec S_ST_POP_Stocks 1,32
```

### n) Stock Adjustment

- `Open`

```sql
Exec S_ST_POP_StockAdjustment 2,1,0,0,''
Exec S_STCON_POP_StoreItems 1,1
Exec S_ST_ItemHelp  1,1
Exec S_STCON_POP_Masters  'T_STO_ItemSubCategory','SubCategoryName,Pk_ItemSubcategoryID','1','1'
```

![alt text](./Stores%20Issue/image-36.png)

- `Search`

```sql
Exec S_ST_POP_StockAdjustment 6,1,0,0,''
Exec S_STCON_POP_Masters  'T_M_Departments','DepartmentName','pk_departmentid','32'
```

![alt text](./Stores%20Issue/image-37.png)

- Select item

```sql
Exec S_ST_POP_StockAdjustment 1,1,5,0,''
```

![alt text](./Stores%20Issue/image-38.png)

- Click `Add`

```sql
Exec S_ST_ItemHelp  2,1,1907
Exec S_STCON_POP_ItemMasterDetails 3,1,0,27
```

![alt text](./Stores%20Issue/image-39.png)

- Click `Save`

```sql
set implicit_transactions on
Exec S_ST_SAVE_StockAdjustment 100,0,14 ,'S','','12/11/2024 10:31:54 AM',405,400,'','5/5/2003','5/15/2003',0,50,50,'RANDAM',10,15,'12/11/2024 10:31:54 AM','12/11/2024 10:31:54 AM',1,37,1
IF @@TRANCOUNT > 0 ROLLBACK TRAN
```

### o) Expiry Items Returns

- `Open`

```sql
EXEC S_SEC_ReturnPermissions 1,1,-1
Exec S_ST_POP_PurIndDepartments 0,1,1,2
Exec S_ST_POP_PurIndTransactions 0,10,1
Exec S_ST_POP_Supplier 5,0,1
Exec S_ST_ItemHelp 1,1
```

![alt text](./Stores%20Issue/image-40.png)

- Select `Category`

```sql
-- SP
Exec S_ST_ItemHelp 2,1,2334
```

- Select `Sub Category`

```sql
-- SP
Exec S_ST_POP_StockForSupplierReturns 313,33
-- Parameter
@StoreID as numeric ,
@SubCateID as numeric
```

### p) Exp Return Tracking

- `Open`

```sql
-- SP
SELECT PK_EXPIRYRETURNID , EXPIRYRETURNNO FROM T_STO_EXPIRYRETURNS WHERE REPLACETRACKED = 0
select * from  SELECT PK_EXPIRYRETURNID , EXPIRYRETURNNO FROM T_STO_EXPIRYRETURNS WHERE REPLACETRACKED = 0

```

![alt text](./Stores%20Issue/image-41.png)

### q) Item Tax Defination

- `Open`

```sql
SELECT PK_TAXID , TAXNAME FROM T_STO_TAXMASTER WHERE ISDELETED = 0
```

![alt text](./Stores%20Issue/image-42.png)

- Enter `Search text`

```sql
SELECT PK_ITEMID , ITEMNAME FROM T_STO_ITEMMASTER WHERE ISDELETED = 0 AND ITEMNAME LIKE 'b%'
```

- Select `Item name`

```sql
SELECT FK_TAXID FROM T_STO_ITEMTAXMAP WHERE ISDELETED = 0 AND FK_ITEMID = 3
```

- `Save`

```sql
UPDATE T_STO_ITEMTAXMAP SET ISDELETED =  1 WHERE FK_ITEMID = 3
INSERT INTO T_STO_ITEMTAXMAP (FK_ITEMID , FK_TAXID , OPERATORID , TERMINALID , HOSPID ) VALUES ( 3 , 6 , 1 , 37 , 1 )
--Delete
UPDATE T_STO_ITEMTAXMAP SET ISDELETED = 1 WHERE FK_ITEMID = 3
```

### r) Tax Master

- `Open`

```sql
SELECT * FROM T_STO_TAXMASTER WHERE ISDELETED = 0
```

![alt text](./Stores%20Issue/image-43.png)

- Click `Tax Name` & `Modify` button

```sql
UPDATE T_STO_TAXMASTER SET TAXNAME = 'VAT 4% INC ON CP' , TAXPERCENT = 4 ,  ISINC = 0 ,  ONMRP = 0 ,  DISBEFORE = 0 ,  OPERATORID = 1 ,  TERMINALID = 37 WHERE PK_TAXID = 2
SELECT * FROM T_STO_TAXMASTER WHERE ISDELETED = 0
```

- `Add` tax

```sql
 INSERT INTO T_STO_TAXMASTER (TAXNAME , TAXPERCENT , ISINC , ONMRP , OPERATORID , TERMINALID)  VALUES  ( 'VAT 1.2',0 , 0 , 0 , 1 , 37 )
 SELECT * FROM T_STO_TAXMASTER WHERE ISDELETED = 0
```

---

---

## 2) Stock Reports

### a) Opening Balance Report

- `Open`

```sql
-- SP
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
-- SP
exec  S_POP_Aesthetics
-- SP
Exec S_ST_POP_PurIndDepartments 0,1,1,5
-- SP
EXECUTE S_POP_FinancialYear @FLAG=5,@PK_FINYEARID=0,@FROMDATE='12:00:00 AM',@TODATE='12:00:00 AM',@HospID=1
```

![alt text](./Stores%20Issue/image-61.png)

- Select `Fin. Year` in `All Stores` & press `Ok`

```sql
--SP
Exec S_ST_REP_OpeningStock @HospID=1 ,@Title='Opening Balance For The Financial Year Apr-1-2014 To Mar-31-2015',@FinID=1,@StoreID=0
```

![alt text](./Stores%20Issue/image-62.png)

Program Terminate

- Select `Fin. Year`, `Store Name` in `Select Stores` & press `Ok`

```sql
Exec S_ST_REP_OpeningStock @HospID=1 ,@Title='Opening Balance For The Financial Year Apr-1-2014 To Mar-31-2015',@FinID=1,@StoreID=32
```

![alt text](./Stores%20Issue/image-63.png)

Program Terminate

### b) Adjustments Report

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndDepartments 0,1,1,2
```

![alt text](./Stores%20Issue/image-64.png)

- Slect `Date`, `Store`, `Surplus` & press `Ok`

![alt text](./Stores%20Issue/image-65.png)

```sql
Exec S_ST_REP_StockAdjustment @HospID=1 ,@Title='Stock Adjustment Surplus Report From 12/Dec/2018 To Date 12/Dec/2024' ,@Operator='SMCH' ,@FromDate='12/12/2018 00:00:00 AM' ,@ToDate='12/12/2024 5:01:02 AM' ,@StoreID=313 ,@AdjustmentType='S'
```

![alt text](./Stores%20Issue/image-66.png)

- Slect `Date`, `Store`, `Defict` & press `Ok`

![alt text](./Stores%20Issue/image-67.png)

```sql
Exec S_ST_REP_StockAdjustment @HospID=1 ,@Title='Stock Adjustment Deficit Report From 12/Dec/2020 To Date 12/Dec/2024' ,@Operator='SMCH' ,@FromDate='12/12/2020 00:00:00 AM' ,@ToDate='12/12/2024 5:06:39 AM' ,@StoreID=313 ,@AdjustmentType='D'
```

![alt text](./Stores%20Issue/image-68.png)

### c) Expiry Items List

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndDepartments 0,1,1,5
```

![alt text](./Stores%20Issue/image-69.png)

- Select `Date`, `Store name` & press `Ok`

![alt text](./Stores%20Issue/image-70.png)

```sql
Exec S_ST_POP_REP_ExpiryItems @HospID=1 ,@Title='Expiry Item Report For The Period From 12 - Dec - 2019 To 12 - Dec - 2024', @fromDate='12/12/2019 5:09:14 AM', @ToDate='12/12/2024 5:09:14 AM' ,@StoreID=32
```

Program terminate

### d) Purchase Orders Report

![alt text](./Stores%20Issue/image-71.png)

### e) Srv Day Book

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_Srv 1,1,0,0
Exec S_ST_POP_PurIndDepartments 0,1,1,5
```

![alt text](./Stores%20Issue/image-72.png)

- Select `Date`, `Transaction Type`, `Store Name` & press `Ok`

![alt text](./Stores%20Issue/image-73.png)

```sql
EXEC S_REP_OPB_hospital 1
Exec S_ST_REP_SrvDayBook @HospID=1 ,@FromDate='12/12/2018 5:14:32 AM' ,@ToDate='12/12/2024 5:14:32 AM' ,@TranID=13 ,@StoreID=32 ,@Title='SRV DayBook For The Period From 12 - Dec - 2018 To 12 - Dec - 2024'
```

![alt text](./Stores%20Issue/image-74.png)

Program Terminate

Select another `Store Name`

![alt text](./Stores%20Issue/image-75.png)

### f) Issue Day Book

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndTransactions 0,5,1
Exec S_ST_POP_PurIndDepartments 0,1,1,5
```

![alt text](./Stores%20Issue/image-76.png)

- Select `Date` & `Store` & press `Report`

![alt text](./Stores%20Issue/image-77.png)

```sql
Exec S_ST_REP_IssueDayBook @HospID=1 ,@FromDate='12/1/2018 5:29:38 AM' ,@ToDate='12/12/2024 5:29:38 AM' ,@TranID=6 ,@StoreID=405 ,@Title='Issue Day Book From Store ICU NURSING For The Period From 01 - Dec - 2018 To 12 - Dec - 2024' ,@DeptID='0'
```

Terminate

### g) Issue Day Book Dept Wise

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Select Getdate() as SystemDate
Select Getdate() as SystemDate
Exec S_ST_POP_PurIndTransactions 0,5,1
Exec S_ST_POP_PurIndDepartments 0,1,1,6
```

![alt text](./Stores%20Issue/image-78.png)

- Select `Date`, `Departmant` & press `Ok`

![alt text](./Stores%20Issue/image-79.png)

```sql
Exec S_ST_REP_IssueDayBook @HospID=1 ,@FromDate='12/25/2015 5:33:02 AM' ,@ToDate='12/12/2024 5:33:02 AM' ,@TranID=6 ,@StoreID=0 ,@Title='Issue Day Book From Store 6TH FLOOR For The Period From 25 - Dec - 2015 To 12 - Dec - 2024' ,@DeptID='313'
```

Terminate

### h) Issue History report

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
Exec S_ST_POP_PurIndDepartments 0,1,1,5
Select Getdate() as SystemDate
Select Getdate() as SystemDate
EXECUTE S_STCON_POP_StoreMapping @FLAG=9,@HospID=1
```

![alt text](./Stores%20Issue/image-80.png)

Select `Category`

```sql
EXECUTE S_STCON_POP_StoreMapping @FLAG=10,@HospID=1,@PARAM1=1907
```

Select `Sub Category`

```sql
EXECUTE S_STCON_POP_StoreMapping @FLAG=3,@HospID=1,@PARAM1=27
```

![alt text](./Stores%20Issue/image-81.png)

Pressw `Ok`

```sql
Exec S_ST_REP_StockLedgerNewONE  1,'Item History Report For The Period From 01 - Dec - 2016 To 12 - Dec - 2024 In CENTRAL STORE For Item                                    ALL PIN',32,318,'12/1/2016','12/12/2024 11:59:59 PM'
select * from  Exec S_ST_REP_StockLedgerNewONE  1,'Item History Report For The Period From 01 - Dec - 2016 To 12 - Dec - 2024 In CENTRAL STORE For Item                                    ALL PIN',32,318,'12/1/2016','12/12/2024 11:59:59 PM'
```

![alt text](./Stores%20Issue/image-82.png)

Terminate

### i) Stock Value On Selling Price

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_STCON_POP_ItemMasterDetails 1,1,0,0
Exec S_ST_ItemHelp 1,1
```

![alt text](./Stores%20Issue/image-83.png)

- Press `Report`

```sql
Exec S_ST_REP_StockValue @HospID=1 ,@Type='S' ,@ItemID=0 ,@Title='Item Wise Stock Value Based On Selling Price As On 12 - Dec - 2024' ,@Operator='SMCH'
```

Program Close

### j) Stock Value On Cost Price

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_STCON_POP_ItemMasterDetails 1,1,0,0
Exec S_ST_ItemHelp 1,1
```

![alt text](./Stores%20Issue/image-84.png)

- Press `OK`

```SQL
Exec S_ST_REP_StockValue @HospID=1 ,@Type='C' ,@ItemID=0 ,@Title='Item Wise Stock Value As On 12 - Dec - 2024' ,@Operator='SMCH'
```

![alt text](./Stores%20Issue/image-85.png)

### k) Stock As On

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndDepartments 0,1,1,5
Exec S_ST_ItemHelp 1,1
```

![alt text](./Stores%20Issue/image-86.png)

- Select `All Stores` & `Category`

```sql
Exec S_ST_REP_StoreWiseStock @HospID=1 ,@Title='Storewise Stock Report As On 12 - Dec - 2024' ,@StoreID=0 ,@FromDate='12/12/2024' ,@ToDate='12/12/2024' ,@CatgID=2214
```

![alt text](./Stores%20Issue/image-87.png)

- Select `Selected Store`, `Store Name`, `Category` & press `Ok`

```sql
Exec S_ST_REP_StoreWiseStock @HospID=1 ,@Title='Storewise Stock Report for CENTRAL STORE As On 12 - Dec - 2024' ,@StoreID=32 ,@FromDate='12/12/2024' ,@ToDate='12/12/2024' ,@CatgID=2309
```

![alt text](./Stores%20Issue/image-88.png)

### l) Stock Wise Sales And Stock

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndDepartments 0,1,1,5
Select Getdate() as SystemDate
Select Getdate() as SystemDate
```

![alt text](./Stores%20Issue/image-89.png)

- Select `Date` & `Store` & press `Ok`

```sql
Exec S_ST_REP_StoreWiseStockPosition @HospID=1 ,@Title='StoreWise Sales And Stock At A Glance Details for PHARMACY From 01 - Dec - 2018 To 12 - Dec - 2024' ,@StoreID=33, @fromDate='12/01/2018 00:00:00 AM', @ToDate='12/12/2024 11:59:59 PM'
```

![alt text](./Stores%20Issue/image-90.png)

### m) Indents Report

- `Open`

```sql
Exec S_ST_POP_PurIndDepartments 0,1,1,2
```

![alt text](./Stores%20Issue/image-91.png)

- Select `Date`, `Dept Name`, `Item Name`. `Detail`

```sql
SELECT ITEMNAME , PK_ITEMID FROM T_STO_ITEMMASTER WHERE ISDELETED =0
```

press `Ok`

```sql
exec S_STO_REP_STOREIDENTSREPORT  '313', '789',1,'2024-12-12 00:00:00','2024-12-12 23:59:59'
```

![alt text](./Stores%20Issue/image-93.png)

Select `Summary` & press `Ok`

```sql
exec S_STO_REP_STOREIDENTSREPORT  '313', '789',1,'2024-12-12 00:00:00','2024-12-12 23:59:59'
```

![alt text](./Stores%20Issue/image-94.png)

- Select `Dept Name` `Check All`

```sql
SELECT ITEMNAME , PK_ITEMID FROM T_STO_ITEMMASTER WHERE ISDELETED =0
```

- Selct `Item Name` `Check All` & press `Ok`

```sql
exec S_STO_REP_STOREIDENTSREPORT  '<ALL>', '<ALL>',1,'2024-12-12 00:00:00','2024-12-12 23:59:59'
```

![alt text](./Stores%20Issue/image-92.png)

### n) ABC Analysis Report

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Select Getdate() as SystemDate
Select Getdate() as SystemDate
Exec S_ST_POP_PurIndDepartments 0,1,1,2
```

![alt text](./Stores%20Issue/image-95.png)

Selct `Store`, `Date` & press `Ok`

```sql
Exec S_ST_REP_ABCAnalysis @HospID=1 ,@Title='ABC Analysis Of 6TH FLOOR From 07/Dec/2018 To Date 12/Dec/2024' ,@Operator='SMCH' ,@FromDate='12/07/2018 00:00:00 AM' ,@ToDate='12/12/2024 6:28:48 AM' ,@StoreID=313
```

![alt text](./Stores%20Issue/image-96.png)

### o) Consumption report

- `Open`

```sql

```

### p) Expiry Returns

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Select Getdate() as SystemDate
Select Getdate() as SystemDate
EXECUTE S_STCON_POP_StoreMapping @FLAG=1,@HospID=1
```

![alt text](./Stores%20Issue/image-97.png)

- Selct `Date`, `Store` & `Supplier`

![alt text](./Stores%20Issue/image-98.png)

```sql
Exec S_ST_POP_REP_SupplierReturnItems @FromDate ='01/Dec/2015 00:00:00 AM' , @ToDate='12/Dec/2024 11:59:59 PM' ,@Title='Expiry Returns For ICU NURSING Store Between 01/Dec/2015 And 12/Dec/2024' ,@StoreId=405 ,@SupplierID =119
select * from Exec S_ST_POP_REP_SupplierReturnItems @FromDate ='01/Dec/2015 00:00:00 AM' , @ToDate='12/Dec/2024 11:59:59 PM' ,@Title='Expiry Returns For ICU NURSING Store Between 01/Dec/2015 And 12/Dec/2024' ,@StoreId=405 ,@SupplierID =119
```

![alt text](./Stores%20Issue/image-99.png)
![alt text](./Stores%20Issue/image-101.png)
![alt text](./Stores%20Issue/image-100.png)

```sql
USE [SANTOSH]
GO
/****** Object:  StoredProcedure [dbo].[S_ST_POP_REP_SupplierReturnItems]    Script Date: 12/12/2024 6:35:17 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO



--- EXEC S_ST_POP_REP_SupplierReturnItems '07/FEB/2008 00:00:01 AM' , '07/FEB/2009 00:00:01 AM' , 33 , 1 , 'SSS'
ALTER Proc [dbo].[S_ST_POP_REP_SupplierReturnItems]
@FromDate as datetime,
@ToDate as datetime,
@StoreId as Numeric=0,
@SupplierID as numeric=0,
@Title as varchar(200)=''

As
set nocount on
BEGIN
declare @strSQL as varchar(1000)
set @strsql ='select ERI.ExpiryReturnNo,ItemName,UomName,BatchNo ,ExpiryDate,ReturnedQty,'
set @strsql =@strsql  + 'CostPrice,(ReturnedQty * CostPrice) as TotalAmount ,SupplierName,'
set @strsql =@strsql  + 'HospitalName,Address1,Address2,ExpiryReturnDate,ReplaceTracked , TrackInformation,''' + @Title + ''''
set @strsql =@strsql  + ' from T_STO_ExpiryReturnItems as ERI,T_STO_ExpiryReturns As ER,'
set @strsql =@strsql  + 'T_STO_ItemMaster IM,T_STO_UOMMaster as UOM,T_STO_SupplierMaster,T_M_HospMaster'
set @strsql =@strsql  + ' where ER.PK_ExpiryReturnID=ERI.FK_ExpiryReturnID and '
set @strsql =@strsql  + 'IM.Pk_ItemID=Fk_ItemID and UOM.PK_UOMID=FK_UOMID and '
set @strsql =@strsql  + 'PK_SupplierId=ER.Fk_SupplierID and PK_HospId=ER.HospID '
set @strsql =@strsql  + ' AND CONVERT ( DATETIME, CONVERT ( VARCHAR , ExpiryReturnDate, 103) , 103 )  >=CONVERT ( DATETIME,'''+ CONVERT ( VARCHAR , @FromDate , 103)+''' , 103 )'
set @strsql =@strsql  + ' AND CONVERT ( DATETIME, CONVERT ( VARCHAR , ExpiryReturnDate, 103) , 103 )  <=CONVERT ( DATETIME,''' + CONVERT ( VARCHAR , @ToDate , 103) + ''', 103 )'

if (@StoreId>0)
Begin
set @strsql =@strsql  + ' and ER.FK_DepartmentID ='+ cast(@StoreId as varchar(10))
end

if (@SupplierID>0)
Begin
set @strsql =@strsql  + ' and ER.Fk_SupplierID ='+ cast(@SupplierID as varchar(10))
end
set @strsql =@strsql  + ' order By SupplierName,ER.PK_ExpiryReturnID,ItemName'
exec (@strSql)
--print @strsql
END
```

### q) Items Below ReOrder

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndDepartments 0,1,1,5
```

![alt text](./Stores%20Issue/image-102.png)

- Select `Store Name` & press `Ok`

```sql
Exec S_ST_REP_StoreWiseReorderQuantity @HospID=1 ,@Title='Re Order Quantity For Store ICU NURSING As On 12/Dec/2024 06:40:28 AM' ,@StoreId=405
```

![alt text](./Stores%20Issue/image-103.png)

### r) Item Master List

- `Open`

```sql
Exec S_ST_POP_OperatorName @HospID=1 ,@OperatorID=1
exec  S_POP_Aesthetics
Exec S_ST_POP_PurIndDepartments 0,1,1,3
Exec S_ST_POP_REP_ItemMasterCategory @Flag=1,@HospID=1 , @ID=0

```

![alt text](./Stores%20Issue/image-104.png)

- Press `Ok`

```sql
Exec S_ST_REP_ItemMasterDetails @HospID=1 ,@StoreID=0 ,@CategoryID=0 ,@SubCategoryID=0 ,@Title='Store Wise Item Master List As On 12-Dec-2024' ,@Operator='SMCH'
```

![alt text](./Stores%20Issue/image-105.png)

- Select `store`

```sql
Exec S_ST_REP_ItemMasterDetails @HospID=1 ,@StoreID=94 ,@CategoryID=0 ,@SubCategoryID=0 ,@Title='Store Wise Item Master List As On 12-Dec-2024' ,@Operator='SMCH'
```

![alt text](./Stores%20Issue/image-106.png)

### s) Srv Summary Report

- `Open`

```sql
 SELECT SUPPLIERNAME , PK_SUPPLIERID FROM T_STO_SUPPLIERMASTER WHERE ISDELETED =0
```

![alt text](./Stores%20Issue/image-107.png)

- Select `Suppler` & press `Ok`

```sql
exec S_ST_REP_SRVVENDORWISE '119' , '2019-12-01 12:00:00 AM' , '2024-12-12 12:00:00 AM'
```

- Select `Check All`

```sql
exec S_ST_REP_SRVVENDORWISE '<ALL>' , '2019-12-01 12:00:00 AM' , '2024-12-12 12:00:00 AM'
```

![alt text](./Stores%20Issue/image-108.png)

### t) Srv Wise Qty

- `Open`

```sql
Select Getdate() as SystemDate
SELECT SUPPLIERNAME , PK_SUPPLIERID FROM T_STO_SUPPLIERMASTER WHERE ISDELETED =0

```

![alt text](./Stores%20Issue/image-109.png)

- Click `Get's`

```sql
SELECT SRVNO , PK_SRVID FROM T_STO_SRV WHERE SRVDATE BETWEEN '12-12-2024' AND '12-12-2024' ORDER BY PK_SRVID
```

- Select `Date`, `Suppler`, `Srv no.` & press `Ok`

```sql
-- Single Selection
exec S_ST_REP_SRVDETAILS '1', '1312','2017-12-01 06:53:02 AM','2024-12-12 06:53:02 AM'
-- All Selection
exec S_ST_REP_SRVDETAILS '<ALL>', '<ALL>','2017-12-01 06:53:02 AM','2024-12-12 06:53:02 AM'
```

![alt text](./Stores%20Issue/image-110.png)

### u) Vender Wise Expiry Items

- `Open`

```sql
Exec S_ST_POP_PurIndDepartments 0,1,1,2
```

![alt text](./Stores%20Issue/image-111.png)

- Select `Date`, `Store` & press `Ok`

```sql
exec S_ST_REP_ExpiryItemsVendorWise  1, ' VENDOR WISE ITEMS EXPIRING BETWEEN  01-Dec-2017 AND 12-Dec-2024' , '2017-12-01 12:00:00 AM ' , '2024-12-12 12:00:00 AM',313
```

![alt text](./Stores%20Issue/image-112.png)

### v) Vender List

![alt text](./Stores%20Issue/image-113.png)

- press `Ok`

```sql
exec S_REP_STO_VENDERDETAILS
```

![alt text](./Stores%20Issue/image-114.png)

### w) Item Wise Details

![alt text](./Stores%20Issue/image-115.png)

### x) Indents Vs issues

- `Open`

```sql
Exec S_ST_POP_PurIndDepartments 0,1,1,2
```

![alt text](./Stores%20Issue/image-116.png)

- Select `Date`, `Dept` & press `Ok`

```sql
-- Single Dept Select
SELECT ITEMNAME , PK_ITEMID FROM T_STO_ITEMMASTER WHERE ISDELETED =0
exec S_REP_ST_IndentVsIssues  '313', '<ALL>','2016-12-01 00:00:00','2024-12-12 23:59:59'
-- Dept Check all
exec S_REP_ST_IndentVsIssues  '<ALL>', '<ALL>','2016-12-01 00:00:00','2024-12-12 23:59:59'
```

![alt text](./Stores%20Issue/image-117.png)
#   O n e S o f t - D o c k 
 
 
