<?php

$connection = mysqli_connect('localhost', 'root', '', 'myproject') or die('No mysqli connection');

$search = @$_REQUEST['search'];
$sql = "SELECT name, date, email, phone FROM orders ORDER BY ";

if(!empty($search))
{
	$sql .= "WHERE (name LIKE '%".mysqli_real_escape_string($connection, $search)."%') OR (email LIKE '%".mysqli_real_escape_string($connection, $search)."%') OR (phone LIKE '%".mysqli_real_escape_string($connection, $search)."%') ";
}

?>

<form method="get">
	<input type="text" name="search" value="<?php echo @$search; ?>"/>
	<button type="submit">Search</button>
</form>


	<?php
				
		$orderby = 'date';
		$sort = 'ASC';
		
if ( isset( $_GET['orderby'] ) and isset( $_GET['sort'] ) ) {
  if ( in_array( $_GET['orderby'], array( 'name', 'date', 'phone' ) ) ) $orderby = $_GET['orderby'];
  if ( in_array( $_GET['sort'], array( 'ASC', 'DESC' ) ) ) $sort = $_GET['sort'];
}

$query = "SELECT name, date, email, phone FROM orders ORDER BY ".$orderby.' '.$sort;
$res = mysqli_query( $connection, $query );
	

echo '<table border="2", width="100%", cellpadding="4" cellspacing="0" style="border-collapse: collapse; empty-cells: show">'."\n";
echo '<tr>';

if ( $sort == 'ASC' ) {
  $tmp = 'DESC';
  $image = 'down.gif';
} else {
  $tmp = 'ASC';
  $image = 'up.gif';
}
if ( $orderby == 'name' )
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=name&sort='.$tmp.'">Client</a>&nbsp;<img src="'.$image.'" alt="" /></th>';
else
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=name&sort=ASC">Client</a></th>';
if ( $orderby == 'date' )
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=date&sort='.$tmp.'">Data</a>&nbsp;<img src="'.$image.'" alt="" /></th>';
else
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=date&sort=ASC">Data</a></th>';
if ( $orderby == 'email' )
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=email&sort='.$tmp.'">Email</a>&nbsp;<img src="'.$image.'" alt="" /></th>';
else
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=email&sort=ASC">Email</a></th>';
if ( $orderby == 'phone' )
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=phone&sort='.$tmp.'">Telephone</a>&nbsp;<img src="'.$image.'" alt="" /></th>';
else
  echo '<th><a href="'.$_SERVER['PHP_SELF'].'?orderby=phone&sort=ASC">Telephone</a></th>';
echo '</tr>'."\n";


while( $prd = mysqli_fetch_array( $res ) ) {
  echo '<tr>'; 
  echo '<td>'.$prd['name'].'</td>'."\n";
  echo '<td>'.$prd['date'].'</td>'."\n";
  echo '<td>'.$prd['email'].'</td>'."\n"; 
  echo '<td>'.$prd['phone'].'</td>'."\n";   
  echo '</tr>'."\n";
}

echo '</table>'."\n";
	
	?>
	</tbody>
