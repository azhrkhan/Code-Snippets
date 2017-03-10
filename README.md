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
    
    // Code to fetch unique record(latest) based on date for two users from a conversation table
╔═════════╦══════════════╦══════╗══════════════╦══════╗
║ to 	  ║  from        ║ msg  ║  date        ║ id   ║
╠═════════╬══════════════╬══════╣══════════════╬══════╣
║  29     ║ 11           ║ hi   ║  123         ║ 1    ║
║  11     ║ 29           ║ bye  ║  345         ║ 2    ║
║  24     ║ 11           ║ oye  ║  678         ║ 3    ║
║  11     ║ 24           ║ hoye ║  911         ║ 4    ║
╚═════════╩══════════════╩══════╝══════════════╩══════╝

The MySQL Query for this is...

SELECT m.*, u1.fullname AS send_toname, u2.fullname as send_fromname FROM
biz_customer u1 CROSS JOIN biz_customer u2
INNER JOIN biz_conversation m ON
u1.customer_id IN (m.send_to, m.send_from)
AND u2.customer_id IN (m.send_to, m.send_from) 
WHERE m.created_at = ( SELECT MAX( t2.created_at ) 
FROM biz_conversation t2 
WHERE ( t2.send_to = m.send_to ) 
OR (t2.send_from = m.send_to)) 
AND u1.customer_id = m.send_to 
AND u2.customer_id = m.send_from
GROUP BY created_at

Will fetch...

╔═════════╦══════════════╦══════╗══════════════╦══════╗═══════════╗═══════════╗
║ to 	  ║  from        ║ msg  ║  date        ║ id   ║to_name    ║from_name  ║
╠═════════╬══════════════╬══════╣══════════════╬══════╣═══════════╣═══════════╣
║  29     ║ 11           ║ hi   ║  345         ║ 2    ║ green     ║ red       ║
║  24     ║ 11           ║ oye  ║  911         ║ 4    ║ white     ║ silver    ║
╚═════════╩══════════════╩══════╝══════════════╩══════╝═══════════╝═══════════╝    
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
