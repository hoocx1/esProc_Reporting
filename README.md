![image](https://www.scudata.com/images/reporting/0.png)

<div align="center">
    <h2>esProc Reporting Solution</h2>
</div>

<div align="center">
    <h3> Technical Architecture</h3>
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/1.png" width="70%" />
</div>

---

<div align="center">
    <h3>Seamless integration with reporting tools/Java applications</h3>
</div>

<div align="center">
    <table style=" border:0; width:100%">
        <tr>
            <td>
                <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/2.png" width="400px" />
            </td> 
            <td>
            <div><strong>esProcPure Java</strong></div>
            <div> ✓ Lightweight</div>
            <div> ✓ Embedded, no independent server</div>
            <div> ✓ JVM，JDK1.8 or above</div>
            <div> ✓ VM/Container/Android</div>
        </tr>
    </table> 
</div>

---

<div align="center">
    <h2>Development Efficiency Improvement</h2>
</div>
 
<div align="center">
    <h3>Easy to develop and debug grid programming</h3>
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/3.png" width="70%" />
</div>

---

<div align="center">
    <h3>Simplify complex SQL of report</h3>
</div>


> **Report objective: To query the status of major customers and orders that account for half of the total sales revenue**

#### SQL
```sql
SELECT CUSTOMER, AMOUNT, SUM_AMOUNT
  FROM (SELECT CUSTOMER, AMOUNT,
    SUM(AMOUNT) OVER(ORDER BY AMOUNT DESC) SUM_AMOUNT
      FROM (SELECT CUSTOMER, SUM(AMOUNT) AMOUNT
           FROM ORDERS GROUP BY CUSTOMER))
           WHERE 2 * SUM_AMOUNT < (SELECT SUM(AMOUNT) TOTAL FROM ORDERS)
```
#### SPL

| | A | B |
| --- | --- | --- |
| 1 | =db.query("select customer,amount from orders order by amount desc") ||
| 2 | =A1.sum(amount)/2	 | =0 |
| 3 | =A1.pselect((B1+=amount)>=A2)	| return A1.to(A3) |

---

<div align="center">
    <h3>More complex SQL</h3>
</div>

> **Report objective: To track the retention of new users for the next day on a daily basis**

#### SQL
```sql
WITH first_login AS ( 
    SELECT userid, MIN(TRUNC(ts)) AS first_login_date  FROM login_data GROUP BY userid),
next_day_login AS (
    SELECT DISTINCT(fl.userid), fl.first_login_date, TRUNC(ld.ts) AS next_day_login_date
    FROM first_login fl LEFT JOIN login_data ld ON fl.userid = ld.userid WHERE TRUNC(ld.ts) = fl.first_login_date + 1),
day_new_users AS (
    SELECT first_login_date,COUNT(*) AS new_user_num FROM first_login GROUP BY first_login_date),
next_new_users AS (
    SELECT next_day_login_date, COUNT(*) AS next_user_num FROM next_day_login GROUP BY next_day_login_date),
all_date AS (
    SELECT DISTINCT(TRUNC(ts)) AS login_date FROM login_data)
SELECT all_date.login_date+1 AS dt,dn. new_user_num,nn. next_user_num,
      (CASE  WHEN nn. next_day_login_date IS NULL THEN 0  ELSE nn.next_user_num END)/dn.new_user_num AS ret_rate
FROM all_date JOIN day_new_users dn ON all_date.login_date=dn.first_login_date
    LEFT JOIN next_new_users nn ON dn.first_login_date+1=nn. next_day_login_date
ORDER BY all_date.login_date;
```

#### SPL

| | A |
| --- | --- |
| 1 | =file("login_data.csv").import@tc() |
| 2 | =A1.group(userid;fst=date(ts):fst\_login,~.(date(ts)).pos(fst+1)>0:w\_sec_login) |
| 3 | =A2.groups(fst_login+1:dt;count(w_sec_login)/count(1):ret_rate) |

---

<div align="center">
    <h3>Enrich report formats</h3>
</div> 


<table style=" border:0; width:100%">
    <tr>
        <td style=" width:40%">
            <img  src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/6.png" >
        </td> 
        <td style="width:60%;">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/6-1.png" >
            <div align="center"><strong>Creating complex reports is simpler than BIRT/JasperReport</strong></div>
        </td> 
    </tr>
</table> 

---
 
<div align="center">
    <h2>Multi Data Source Computing</h2>
</div>

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:40%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/7.png"  width="400px">
        </td> 
        <td style=" width:60%">
            <h3>Multi data source report</h3>
            <div> ✓ Access data sources directly through the native interface, without predefined mapping, while preserving their features</div>
            <div> ✓ Lightweight, avoiding heavy logical data warehouses</div>
        </td> 
    </tr>
</table> 

<div align="center">
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/7-2.png"  width="80%">
</div>
 
 
---

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:60%">
            <h3> Application of multi source mixed computing - Real-time report</h3>
            <div> ✓ AHistorical cold data is calculated and read from the AP database</div>
            <div> ✓ Transaction hot data is read from the TP database in real time</div>
            <div> ✓ Mixed computing to implement real-time report of whole dat </div>
        </td> 
        <td style=" width:40%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/8.png"  width="400px">
        </td> 
    </tr>
</table> 
 
---

<div align="center">
    <h3>Cross database migration</h3>
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/9.png"  width="70%">
    <div><strong>One SQL statement that works across all databases, allowing migration without altering the report.</strong></div>
</div>

---

<div align="center">
    <h3>Cross database migration – Conversion example</h3> 
</div>


|  | |
| --- | --- |
| **SQL** | SELECT EID, NAME, BIRTHDAY, **ADDMONTHS(BIRTHDAY,10)** DAY10 FROM EMP |
| | **⇩ esProc conversion ⇩** | 
| **ORACLE** | SELECT EID, NAME, BIRTHDAY, **BIRTHDAY+NUMTOYMINTERVAL(10,'MONTH')** DAY10 FROM EMP| 
| **SQLSVR** | SELECT EID, NAME, BIRTHDAY, **DATEADD(MM,10,BIRTHDAY)** DAY10 FROM EMP| 
| **DB2** | SELECT EID, NAME, BIRTHDAY, **BIRTHDAY+10 MONTHS** DAY10 FROM EMP| 
| **MYSQL** | SELECT EID, NAME, BIRTHDAY, **BIRTHDAY+INTERVAL 10 MONTH** DAY10 FROM EMP| 
| **POSTGRES** | SELECT EID, NAME, BIRTHDAY, **BIRTHDAY+interval '10 months'** DAY10 FROM EMP| 
| **TERADATA** | SELECT EID, NAME, BIRTHDAY, **ADD_MONTHS(BIRTHDAY, 10)** DAY10 FROM EMP| 

---

<div align="center">
    <h3>The file data source</h3> 
</div>

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:50%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/11.png"  width="400">
        </td> 
        <td style=" width:50%">
            <img src="https://www.scudata.com/images/reporting/11-1.png" width="500">
        </td> 
    </tr>
</table> 
 
---

<div align="center">
    <h2>Architecture Optimization</h2> 
</div>



<table style=" border:0; width:100%">
    <tr>
        <td style=" width:40%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/12.png"  width="400">
        </td> 
        <td style=" width:60%">
            <h3>Report microservices</h3>
            <div> ✓ "Report data sources are delivered as microservices, simplifying extension and migration</div>
            <div> ✓ esProc scripts are interpreted, enabling hot swapping by default</div>
            <div> ✓ Adapt to the ever-changing reporting business.</div>
        </td> 
    </tr>
</table> 

---

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:40%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/13.png"  width="400">
        </td> 
        <td style=" width:60%">
            <h3>Replace stored procedures</h3>
            <div> ✓ Move stored procedures outside the database, making them independent and easily migratable</div>
            <div> ✓ Reduce coupling between applications</div>
            <div> ✓ No need for compile privileges on stored procedures, enhancing security and reliability</div>
        </td> 
    </tr>
</table> 

---

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:40%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/14.png"  width="400">
        </td> 
        <td style=" width:60%">
            <h3>Eliminate intermediate tables in the database</h3>
            <div> ✓ Move intermediate tables to files for storage and processing, easing the database load</div>
            <div> ✓ The tree-structured file system simplifies management and reduces coupling between applications</div>
        </td> 
    </tr>
</table> 

---

<div align="center">
    <h2>Performance Boosting</h2> 
</div>

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:50%">
            <h3>Multi threaded parallel data retrieval</h3>
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/15.png"  width="400">
            <div><strong>Multi threaded parallel data retrieval, compatible with both homogeneous and heterogeneous databases, boosts query performance</strong></div>
        </td> 
        <td style=" width:50%">
            <div>Single table parallel data retrieval:</div>
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/15-1.png"  >
            <div>Parallel data retrieval from multiple tables (databases):</div>
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/15-2.png"  >
        </td> 
    </tr>
</table> 

---

<div align="center">
    <h3>Simple Big Data and Parallel Computing</h3>
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/16.png"  >
    <div><strong>An option to enable parallelism can significantly enhance computing performance</strong></div>
</div>

---

<table style=" border:0; width:100%">
    <tr>
        <td style=" width:40%">
            <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/17.png"  width="400px">
        </td> 
        <td style=" width:60%">
            <h3>Local file data cache</h3>
            <div> ✓ Faster file system IO</div>
            <div> ✓ Binary format does not require parsing</div>
            <div> ✓ High performance support: compression, columnar storage, indexing …</div>
        </td> 
    </tr>
</table> 

---

<div align="center">
    <h3>High performance algorithms</h3>
    <img src="https://www.scudata.com/wp-content/themes/scudata-en/images/Langding-Page-esProc-for-Reporting/18.png"  >
    <div><strong>Improve the performance of report computation</strong></div>
</div>

---

# Resource

- [Download esProc SPL](https://www.scudata.com/download-esproc)
- [Community - c.scudata.com](https://c.scudata.com/)