import mysql.connector 
from mysql.connector import errors

try:
    conn = mysql.connector.connect(host="localhost",user="root",password="", database="exo01")
    conn.autocommit = False
    cursor = conn.cursor()
    
    query_1 = "SELECT * FROM departement WHERE departement_id = 6 OR departement_id = 8"
    cursor.execute(query_1)
    
    data = cursor.fetchall()
    print(data)

    conn.start_transaction()
    sql_update_query_1 = "UPDATE departement SET departement_nom = 'TEST TRANSACTION' WHERE departement_id = 6"
    sql_update_query_2 = "UPDATE departement SET departement_nom = 'TEST TRANSACTION 2' WHERE departement_id = 8"

    cursor.execute(sql_update_query_1)
    cursor.execute(sql_update_query_2)
    
    #conn.commit() 
    conn.rollback()
    
    
    query_2 = "SELECT * FROM departement WHERE departement_id = 6 OR departement_id = 8"
    cursor.execute(query_1)
    
    data_2 = cursor.fetchall()
    print(data_2)

except errors.Error as e:
    conn.rollback()  # rollback changes
    print("Rolling back ...")
    print(e)


conn.close()
