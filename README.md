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
    
    // Code to select the maximum record for each column with the same value in Codeigniter
    
    $this->db->select('MAX(created_at)')
         ->from('biz_conversation')
         ->group_by('send_to');
		 $subquery = $this->db->get_compiled_select();

		$this->db->select('*')
         ->from('biz_conversation')
         ->where("created_at  IN($subquery)", NULL, FALSE)
         ->get()
         ->result();
