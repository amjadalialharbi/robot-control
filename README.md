# robot-control
A web-based robot control project using PHP, with directions stored in a MySQL database via XAMPP. It allows users to send movement commands to the robot and view the last stored directions.
## Step 1: Install XAMPP
1. Download XAMPP 
2.  Open the XAMPP Control Panel and start the Apache and MySQL modules.
## Step 2: Create the Database
1.Open The web browser and go to http://localhost/phpmyadmin.

2.Create a database named `robot_control`

## Step 3: Create the Table
-Create a table named ` directions `using the following SQL code:

```sql
CREATE TABLE directions (
  id int(11) NOT NULL AUTO_INCREMENT,
  direction varchar(50) NOT NULL,
  PRIMARY KEY (`id`)
);
```
## Step 4: Set Up the Project Files
1-Navigate to the XAMPP htdocs directory ( `C:\xampp\htdocs`).

2-Create a new folder named  `robot.`

## Step 5: Create the Control Page

1. Inside the robot folder, create a new file named ` index.php`.
2. Add the following code to` index.php`:
```
<!DOCTYPE html>
<html>
<head>
    <title>Robot Control</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }

        h {
            margin-top: 22px;
            font-size: 33px;
        }
        
        .controller {
            text-align: center;
        }
        
        .controller button {
            margin: 30px 30px;
            padding: 15px;
            font-size: 15px;
            background-color: #158ee5;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h>Robot Control:</h>
    <div class="controller">
        <form action="control.php" method="post">
            <button type="submit" name="direction" value="forward">Forward</button><br>
            <button type="submit" name="direction" value="left">Left</button>
            <button type="submit" name="direction" value="stop">Stop</button>
            <button type="submit" name="direction" value="right">Right</button><br>
            <button type="submit" name="direction" value="backward">Backward</button>
        </form>
    </div>
</body>
</html>
```
### Step 6: Create the Control Script

1. Inside the robot folder, create a new file named `control.php`.
2. Add the following code to `control.php`:

```
<?php

$servername = "localhost";
$username = "root"; 
$dbname = "robot_control";

$conn = new mysqli($servername, $username, $password, $dbname);

// 
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

// 
if (isset($_POST['direction'])) {
    $direction = $_POST['direction'];

    // 
    $sql = "INSERT INTO directions (direction) VALUES ('$direction')";

    if ($conn->query($sql) === TRUE) {
        echo "Direction saved successfully";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}

$conn->close();

?>;
```
## Step 7: Create the View Page

1. Inside the robot folder, create a new file named `last_direction.php`.
2. Add the following code to `last_direction.php`:
```
<!DOCTYPE html>
<html>
<head>
    <title>Last Direction</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
            margin: 0;
            padding: 0;
        }

        h1 {
            margin-top: 22px;
            font-size: 33px;
            
        }

        .data {
            margin-top: 20px;
            font-size: 20px;
        }

        .back-button {
            margin-top: 20px;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            background-color: #158ee5;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Last Direction Stored</h1>
    <div class="data">
        <?php
        
        $servername = "localhost";
        $username = "root"; 
        $password = ""; 
        $dbname = "robot_control";

        
        $conn = new mysqli($servername, $username, $password, $dbname);
        if ($conn->connect_error) {
            die("Connection failed: " . $conn->connect_error);
        }
        $sql = "SELECT direction FROM directions ORDER BY id DESC LIMIT 1";
        $result = $conn->query($sql);

        if ($result->num_rows > 0) {
            
            $row = $result->fetch_assoc();
            echo "<p>Direction: " . $row["direction"] . "</p>";
        } else {
            echo "<p>No directions found.</p>";
        }

        $conn->close();
        ?>
    </div>
    <div class="back-button">
        <form action="index.php" method="get">
            <button type="submit">Back to Control Page</button>
        </form>
    </div>
</body>
</html>
```
# Control interface:
![amg2](https://github.com/amjadalialharbi/robot-control/assets/174282482/0fe9490b-a2c2-4c0a-a56f-78485e7f1cd8)

# Database Integration:
![amg1](https://github.com/amjadalialharbi/robot-control/assets/174282482/860c33fe-8055-4407-a45d-a63de2796b77)


