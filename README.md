# Pointermap
With this website base you can:
Add markers to Google maps base
View markers without messing with them
Search markes by address

This site uses PHP echo to echo markers to XML file, and website will read that file to show markers.

# TO BE NOTED
You can't delete markers using this project, markers can only be deleted via mysql/phpmyadmin

# Deploying website
Add api key to following: CreateMarkers.html and index.html

Need to edit following:
# CreateMarkers.html
```
$connection=mysql_connect ('DATABASENAME', $username, $password);'
```
Add your database name
```
<tr><td>Name:</td><td><input type='text' id='name'/></td> </tr>
      <tr><td>Address:</td><td><input type='text' id='address'/></td></tr>
      <tr><td>Subject:</td><td><input type='text' id='subject'/></td></tr>
      <tr><td>Type:</td> <td><select id='type'> +
                 <option value='Subject1' SELECTED>subject1</option>
                 <option value='Reference'>Reference</option>
```
Here you can edit what information you want to save
# getmarkers.php
```
$connection=mysql_connect ('DATABASENAME', $username, $password);
```
Add your database name, again
```
$query = "SELECT * FROM markers WHERE 1";
```
Add datatable name example "markers" i recommend using markers name for table, but if you have different name change it here
```
echo 'id="' . $ind . '" ';
  echo 'name="' . parseToXML($row['name']) . '" ';
  echo 'address="' . parseToXML($row['address']) . '" ';
  echo 'subject="' . parseToXML($row['subject']) . '" ';
  echo 'lat="' . $row['lat'] . '" ';
  echo 'lng="' . $row['lng'] . '" ';
  echo 'type="' . $row['type'] . '" ';
```
If you edited what is being saved, now you need to edits same id's here
# index.html
```
 Array.prototype.forEach.call(markers, function(markerElem) {
              var id = markerElem.getAttribute('id');
              var name = markerElem.getAttribute('name');
              var address = markerElem.getAttribute('address');
              var subject = markerElem.getAttribute('subject');
              var type = markerElem.getAttribute('type');
              var point = new google.maps.LatLng(
                  parseFloat(markerElem.getAttribute('lat')),
                  parseFloat(markerElem.getAttribute('lng')));
```
This is what will be echoed to XML file, same edit here that before.
```
var infowincontent = document.createElement('div');
              var strong = document.createElement('strong');
              strong.textContent = name
              infowincontent.appendChild(strong);
              infowincontent.appendChild(document.createElement('br'));
              var text = document.createElement('text');
              text.textContent = address
              infowincontent.appendChild(text);
              infowincontent.appendChild(document.createElement('br'));
              var text = document.createElement('text');
              text.textContent = subject
              infowincontent.appendChild(text);
              infowincontent.appendChild(document.createElement('br'));
```
This is what is shown on infowindows when marker is cliked
# markers.php
```
$name = $_GET['name'];
$address = $_GET['address'];
$subject = $_GET['subject'];
$lat = $_GET['lat'];
$lng = $_GET['lng'];
$type = $_GET['type'];
```
same edits in here than before
```
$connection=mysql_connect ("MYSQLSERVERNAME", $username, $password);
```
Also add mysql server name here
```
$query = sprintf("INSERT INTO markers " .
         " (id, name, address, subject, lat, lng, type ) " .
         " VALUES (NULL, '%s', '%s', '%s', '%s', '%s', '%s');",
         mysql_real_escape_string($name),
         mysql_real_escape_string($address),
         mysql_real_escape_string($subject),
         mysql_real_escape_string($lat),
         mysql_real_escape_string($lng),
         mysql_real_escape_string($type));
```
If you added things that is being saved, add ``` '%s', ``` per item saved, also add string for it.
# settings.php
```
$username="USER";
$password="PASSWORD";
$database="DATABASE";
```
Here you add credentials for login, ```$database="DATABASE";``` if you use remote mysql server, add address of that server example ```$database="www.myremotedatabase.com"; ``` if database is local use ```$database="localhost";``` or just ```$database="local_database_name"; ```
