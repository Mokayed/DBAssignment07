<h1 align="center">DBAssignment07</h1>

<p>This is a solution for the following databases assignment: https://github.com/datsoftlyngby/soft2019spring-databases/blob/master/assignments/Assignment7.md</p>

<h1>Setup <g-emoji class="g-emoji" alias="checkered_flag" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f3c1.png">🏁</g-emoji></h1>

<p>In order to create the "CustomerOverview" materialized use the following query:</p>
  
```sql
  CREATE 
    ALGORITHM = UNDEFINED 
    DEFINER = `root`@`%` 
    SQL SECURITY DEFINER
VIEW `CustomerOverview` AS
    SELECT 
        `customers`.`customerNumber` AS `customerNumber`,
        `customers`.`customerName` AS `customerName`,
        CONCAT(`customers`.`contactFirstName`,
                ' ',
                `customers`.`contactLastName`) AS `contactName`,
        `customers`.`phone` AS `contactPhone`,
        `customers`.`city` AS `custCity`,
        `customers`.`postalCode` AS `custZip`,
        `customers`.`country` AS `custCountry`,
        CONCAT(`employees`.`firstName`,
                ' ',
                `employees`.`lastName`) AS `repName`,
        `employees`.`email` AS `repEmail`,
        `offices`.`phone` AS `repPhone`,
        `offices`.`postalCode` AS `repZip`,
        `offices`.`city` AS `repCity`,
        CONCAT(`offices`.`addressLine1`,
                '
                ',
                `offices`.`addressLine2`) AS `repAddress`,
        `offices`.`country` AS `repCountry`
    FROM
        ((`employees`
        JOIN `customers` ON ((`employees`.`employeeNumber` = `customers`.`salesRepEmployeeNumber`)))
        JOIN `offices` ON ((`employees`.`officeCode` = `offices`.`officeCode`)))
```
<h1>Exercises<g-emoji class="g-emoji" alias="page_with_curl" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/1f4c3.png">📃</g-emoji></h1>

<h2>Exercise 1</h2>


 <table>
<thead>
<tr>
<th align="left">Colums</th>
<th align="center">1_customerNumber</th>
<th align="center">2_customerName</th>
<th align="center">3_contactName</th>
<th align="center">4_contactPhone</th>
<th align="center">5_custCity</th>
<th align="center">6_custZip</th>
<th align="center">7_custCountry</th>
 <th align="center">8_repName</th>
 <th align="center">9_repEmail</th>
 <th align="center">10_repPhone</th>
 <th align="center">10_repZip</th>
 <th align="center">11_repCity</th>
 <th align="center">12_repAddress</th>
 <th align="center">13_repCountry</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left"><strong>Depends</strong></td>
<td align="center">1</td>
<td align="center">1</td>
<td align="center">1</td>
<td align="center">1,3</td>
<td align="center">1</td>
<td align="center">5</td>
<td align="center">5,6</td>
<td align="center">1,3 (not sure)</td>
<td align="center">8</td>
<td align="center">8</td>
<td align="center">10</td>
<td align="center">11</td>
<td align="center">10,11</td>
<td align="center">12</td>

</tr>
</tbody>
</table>

<p>We conclude the view violates a first normal. We conclude also that the view violates a secound normal form, because there is some columns in the view table that contains multiple values. At the same time the view violtes a third normal form because non-key attribute depends on another non-key attribute you can see the on repAddress depends on custZip and custCity.</p>

<h2>Exercise 2</h2>
We would make a primary key which contains the customerName and repEmail as a composite key, since custCity, custZip, custCountry do not depend on the repEmail of the composite key, and repName, repPhone, repZip, repCity, repAddress, repCountry don't depend on customerNamepart key. With the creation of the composite key there would be violated more usage of the second normal form.

<h2>Exercise 3</h2>

<p>We created a full text indext on the phone in offices in order to not get the "you are using safe update mode" error.
indexes are function based that avoids the implicit type conversion. down below is the code:</p>

```sql
CREATE FULLTEXT INDEX index_name
ON posts(title)
```
  
  <p>
  Then we were able to change the repPhone (first line may not be necessary, try using only last line first, because the offices table should also be affected by that line):</p>
 
 ```sql
  update offices set phone = "+1 212 555 4000" where (phone = "+1 212 555 3000" AND officeCode <> 0);
update CustomerOverview set repPhone = "+1 212 555 4000" where (repPhone in ("+1 212 555 3000") AND customerNumber <> 0);
  ```
  
  <h3>Exercise 3.1</h3>
  
  <p>We first tried out this line of code to see what would happen:</p>
  
   ```sql
  update CustomerOverview set repEmail = "newEmail@classicmodelcars.com" where repName = "Leslie Jennings";
  ```
  
  <p>Our expectation to this line of code was that only the view would be affected, but to our surprise the employees table was also updated.
  Then we started to research where thinsg could go wrong and we found out that some of the "rep's" were having the same phone numbers. (example: leslie jennings and leslie thompson), so they are in family which could cause issues if updating by the number. So a silly way of doing it would be this:</p>
 
  ```sql
  set repEmail = "newEmail@classicmodelcars.com" where repPhone = "+1 650 219 4782";
   ```
  
<h2>Exercise 4</h2>
<p>This is an image that represents an index assigned to customerName, and the rowId.</p>
  <img src="https://github.com/Hallur20/DBAssignment07/blob/master/tegning.png"/>
  
