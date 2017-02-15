# Code-Snippets
Code to create a JSON array form an associative array of MySQL result in codeigniter

For an associative array of this type, we could have this code in PHP for creating a JSON array meant for datatables (jQuery)

     $getSessionList = array(

     '0' =>array(

     'name' => 'azure'

          ),
     '1' =>array(

     'name' => 'maroon'

          )
      );

     $tableData = array();
     $tableData['data'] = array_map(function($getSessionList) {
    return array($getSessionList['name']);
    }, $getSessionList);

    // any associative array converts to js-object
    // it doesn't matter is that an associative array or std-object
    echo json_encode($tableData);
    exit;
