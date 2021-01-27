# Update Transactions - Code Examples

* Run the  INSERT command using the Fabric user code as follows:

~~~
Db ci = db("fabric");

// Get the instance ID 
ci.execute("get Customer.'" + IID + "'");

// Begin Trx
ci.beginTransaction();

//Perform the INSERT command
String SQL = "INSERT into CONTRACT_COPY (CUSTOMER_ID,CONTRACT_ID,CONTRACT_REF_ID) values (?, ?, ?)";
Object[] params = new Object[]{IID, contrID, contrRefID};
ci.execute(SQL, params);

//Commit Trx
ci.execute("commit");
~~~



* Run the GET and INSERT commands from the Web Service within the same transaction as follows:

~~~
String getCommand = "get " + LU_NAME + ".'" + UID + "'";
Db ci = db("fabric");
String status = "SUCCESS";

try {
    ci.execute(getCommand);         	
    String sql = "SELECT COUNT(*) NO_OF_ACT FROM ACTIVITY";	
    ludb(LU_NAME, UID).fetch(sql).each(row->{
	ci.beginTransaction();
	Object [] params = new Object[]{UID, 99, 0, "NA", row.cell(0) + " total num of activities for the customer"};
	//log.info("*-*-* Insert records into ACT_CASE_NOTE" );
	ci.execute("INSERT INTO ACT_CASE_NOTE (CUSTOMER_ID, ACTIVITY_ID, CASE_ID, CASE_NOTE_ID, CASE_NOTE_TEXT) VALUES (?, ?, ?, ?, ?)", params);
	ci.commit();
    });		
} catch (Exception e) {
    log.info("Insert failed: " + e.getMessage());
    ci.rollback();
    status = "FAILURE";	
}
~~~



* Run the transaction using the Fabric Console as follows:

~~~
 // get the instance ID  
 get Customer.22;
	
 // Begin Trx
 begin;

 // Run the UPDATE command
 update CONTRACT_COPY set CONTRACT_REF_ID = 'AXD1234' where CUSTOMER_ID = 22;	
    
 // Commit Trx
 commit;
~~~



- Run the Reference update transaction:

~~~
Db ci = db(“fabric”);
ci.beginTransaction(); 
ci.execute("INSERT INTO REF_STATUS (STATUS_CODE, STATUS_DESC) VALUES (?,?)", i_cd, i_desc);
ci.commit();
~~~





[![Previous](/articles/images/Previous.png)](/articles/23_fabric_transactions/02_fabric_transactions.md)
