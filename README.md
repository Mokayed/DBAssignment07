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
<td align="left"><strong>Dependent</strong></td>
<td align="center">1</td>
<td align="center">2</td>
<td align="center">3</td>
<td align="center">4</td>
<td align="center">5</td>
<td align="center">4</td>
<td align="center">4</td>
<td align="center">4</td>
<td align="center">4</td>
<td align="center">4</td>
<td align="center">4</td>
<td align="center">4</td>
<td align="center">4</td>

</tr>
</tbody>
</table>


